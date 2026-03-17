---
description: The official TestBox CLI for CommandBox — run, scaffold, watch, and manage your tests from the command line.
icon: square-terminal
metaLinks:
  alternates:
    - https://app.gitbook.com/s/06sPF32m7jrKRFAXmFkA/getting-started/testbox-cli
---

# TestBox CLI

The **TestBox CLI** is a [CommandBox](https://www.ortussolutions.com/products/commandbox) module that brings test execution, scaffolding, watching, and project management directly to your terminal. It is the recommended way to interact with TestBox from the command line — especially for BoxLang projects.

## System Requirements

* CommandBox **6.x+**

## Installation

```bash
box install testbox-cli
```

Once installed, all `testbox` commands are available in your CommandBox shell. Use `--help` on any command to explore its options:

```bash
testbox run --help
testbox create bdd --help
testbox generate harness --help
```

---

## 🔍 Language Detection

The CLI automatically detects whether your project targets **BoxLang** 🟦 (primary) or **CFML** so that generated files use the correct syntax. Detection is applied in this order:

### BoxLang Detection

1. The server running in the current directory is a BoxLang server
2. `box.json` contains `testbox.runner=boxlang`
3. `box.json` contains `language=boxlang`

### CFML Detection (fallback)

1. The server running in the current directory is a CFML server (Lucee or Adobe ColdFusion)
2. `box.json` contains `language=cfml`

You can always override detection for any generator command using `--boxlang` or `--noBoxlang`.

---

## ▶️ Running Tests

The `testbox run` command executes your TestBox suite via HTTP and renders formatted results in the CLI.

### Basic Usage

```bash
# Uses testbox.runner from box.json
testbox run

# Pass a full URL directly
testbox run http://localhost:8080/tests/runner.cfm

# Use a relative path (requires a running CommandBox server in the current directory)
testbox run /tests/runner.cfm
```

### Named Runners

Define multiple named runner targets in `box.json` for different environments:

```json
"testbox": {
    "runner": [
        {
            "dev": "http://localhost:8080/tests/runner.cfm",
            "staging": "http://staging.myapp.com/tests/runner.cfm"
        }
    ]
}
```

Then target a specific runner by name:

```bash
testbox run dev
testbox run staging
```

### Filtering Tests

```bash
# Run a specific bundle
testbox run bundles=tests.specs.MyBDDSpec

# Run tests from a directory (dot notation)
testbox run directory=tests.specs

# Target a specific suite
testbox run testSuites="My Suite"

# Target a specific spec
testbox run testSpecs="it should do something"

# Filter by labels
testbox run labels=unit
testbox run labels=integration excludes=slow
```

### Verbose Output

By default only failing test details are shown. Use `--verbose` to include passing and skipped specs:

```bash
testbox run --verbose
```

### Ad-hoc URL Options

Pass arbitrary URL parameters to the runner, either inline or persisted in `box.json`:

```bash
# Inline
testbox run options:foo=bar options:env=dev

# Persisted
package set testbox.options.foo=bar
package set testbox.options.env=dev
```

### Output Formats for CI/CD

Produce multiple report formats from a single test run — all derived from the same JSON result:

```bash
# Generate JSON and ANT JUnit reports
testbox run outputFormats=json,antjunit

# With a custom output file name
testbox run outputFormats=json,antjunit,simple outputFile=myresults
```

Available formats: `json`, `xml`, `junit`, `antjunit`, `simple`, `dot`, `doc`, `min`, `mintext`, `text`, `tap`, `codexwiki`

### 🌊 Streaming Mode

Stream each spec result in real-time via Server-Sent Events (SSE) — no waiting for the full suite to finish:

```bash
testbox run --streaming
```

{% hint style="info" %}
Streaming requires your TestBox runner to support SSE output. This is available natively when running TestBox on BoxLang or a compatible CFML server.
{% endhint %}

### Configuring Defaults in `box.json`

Store your preferences in `box.json` so they apply automatically on every run:

```bash
package set testbox.runner=http://localhost:8080/tests/runner.cfm
package set testbox.verbose=true
package set testbox.labels=unit
package set testbox.testSuites=MySuite
```

---

## 👀 Watching Tests

The `testbox watch` command monitors your source files and automatically re-runs your tests whenever a file changes — great for a tight red/green/refactor loop.

### Basic Usage

```bash
# Start the watcher (uses testbox.runner from box.json)
testbox watch
```

On each detected change the screen clears and `testbox run` fires with your configured options. Press **Ctrl+C** to stop the watcher.

### Watch Options

```bash
# Watch specific file glob patterns
testbox watch paths=models/**.bx,tests/**.bx

# Adjust the polling delay (ms, minimum 150ms)
testbox watch delay=1000

# Scope to a directory or bundles
testbox watch directory=tests.specs
testbox watch bundles=tests.specs.MySpec

# Filter by labels
testbox watch labels=unit
```

### Configuring the Watcher in `box.json`

```bash
package set testbox.watchPaths=/models/**.bx,/tests/**.bx
package set testbox.watchDelay=1000
package set testbox.verbose=false
package set testbox.labels=unit
```

{% hint style="warning" %}
Make sure your server is already running and `testbox.runner` is configured in `box.json` before starting the watcher.
{% endhint %}

---

## 🏗️ Scaffolding

The `testbox create` commands scaffold new test files in either BoxLang or CFML based on your project's [language detection](#language-detection).

### Create a BDD Spec

```bash
# Create in the current directory
testbox create bdd MySpec

# Create in a specific directory
testbox create bdd MySpec directory=tests/specs

# Using dot notation for nested paths
testbox create bdd myPackage.MySpec

# Open the file in your editor after creation
testbox create bdd MySpec --open

# Force a specific language
testbox create bdd MySpec --boxlang
testbox create bdd MySpec --noBoxlang
```

**Generated BoxLang BDD Spec (`MySpec.bx`):**

```js
/**
 * My BDD Test
 */
class extends="testbox.system.BaseSpec" {

    function beforeAll() { }
    function afterAll() { }

    function run( testResults, testBox ) {
        describe( "My First Suite", () => {

            it( "A Spec", () => {
                fail( "implement" );
            } )

        } )
    }

}
```

### Create an xUnit Test

```bash
# Create in the current directory
testbox create unit MyTest

# Create in a specific directory
testbox create unit MyTest directory=tests/specs

# Using dot notation for nested paths
testbox create unit myPackage.MyTest

# Open the file in your editor after creation
testbox create unit MyTest --open
```

**Generated BoxLang xUnit Test (`MyTest.bx`):**

```js
/**
 * My xUnit Test
 */
class extends="testbox.system.BaseSpec" {

    function beforeTests() { }
    function afterTests() { }
    function setup( currentMethod ) { }
    function teardown( currentMethod ) { }

    @Test
    @DisplayName "A beautiful xUnit Test"
    function myMethodTest() {
        fail( "implement it" )
    }

}
```

---

## 🧪 Generating a Test Harness

The `testbox generate harness` command scaffolds a complete `tests/` directory for your project, including a runner, `Application.bx`/`Application.cfc`, and a sample spec — everything you need to start testing immediately.

```bash
# Generate in the current project
testbox generate harness

# Generate for a specific project path
testbox generate harness /path/to/myApp

# Force a specific language
testbox generate harness --boxlang
testbox generate harness --noBoxlang
```

{% hint style="info" %}
If TestBox is not already installed locally, the CLI will install it automatically before scaffolding the harness.
{% endhint %}

---

## 🌐 Generating a Test Browser

The `testbox generate browser` command creates a web-based test browser UI at `tests/browser/` — a convenient, visual way to browse and execute your test bundles from a browser.

```bash
# Generate in the current project
testbox generate browser

# Generate for a specific project path
testbox generate browser /path/to/myApp

# Force BoxLang
testbox generate browser --boxlang
```

The browser UI is placed at `tests/browser/` and can be accessed via your web server once your server is running.

---

## 📊 Generating a Test Visualizer

The `testbox generate visualizer` command generates a standalone HTML-based test results visualizer at `tests/test-visualizer/`. Load a JSON results file to get a rich visual breakdown of any test run.

```bash
# Generate in the current project
testbox generate visualizer

# Generate for a specific project path
testbox generate visualizer /path/to/myApp
```

**Using the visualizer:**

1. Run your tests with JSON output: `testbox run outputFormats=json outputFile=test-results`
2. Open `tests/test-visualizer/index.html` in your browser
3. Load the generated `test-results.json` file

---

## 📦 Generating a TestBox Module

The `testbox generate module` command scaffolds a new TestBox extension module — ideal for packaging custom matchers, reporters, or shared test helpers.

```bash
# Create a module in the current directory
testbox generate module myModule

# Create the module in a specific directory
testbox generate module myModule tests/resources/modules

# Force a specific language
testbox generate module myModule --boxlang
```

---

## 🔧 Installation Management

### View TestBox Info

Display which TestBox installation the CLI is currently using — local project copy or the CLI-bundled copy:

```bash
testbox info
```

The output shows the active version, the path on disk, and its source. If no TestBox is found anywhere, you will be prompted to install it.

### Reinstall the CLI Bundle

The CLI ships with its own bundled copy of TestBox used for output formatting and report generation. If this copy is outdated or corrupted, reinstall it:

```bash
# Reinstall the latest stable version
testbox reinstall

# Reinstall a specific version
testbox reinstall version=5.3.0

# Skip the confirmation prompt
testbox reinstall --force
testbox reinstall version=5.3.0 --force
```

{% hint style="warning" %}
`testbox reinstall` only affects the CLI-bundled copy of TestBox. It does **not** touch any TestBox installation inside your project.
{% endhint %}

---

## 📚 Documentation & Resources

Open the TestBox docs or API reference directly from the CLI:

```bash
# Open the TestBox documentation site
testbox docs

# Search the docs for a specific topic
testbox docs search=matchers
testbox docs search=mocking

# Open the TestBox API docs
testbox apidocs
```

---

## ⚡ Quick Reference

| Command | Description |
|---------|-------------|
| `testbox run` | Execute tests via HTTP runner |
| `testbox run --streaming` | Stream test results in real-time via SSE |
| `testbox watch` | Watch files and re-run tests on change |
| `testbox create bdd <name>` | Scaffold a new BDD spec |
| `testbox create unit <name>` | Scaffold a new xUnit test |
| `testbox generate harness` | Generate a full `tests/` harness |
| `testbox generate browser` | Generate a web-based test browser UI |
| `testbox generate visualizer` | Generate an HTML test results visualizer |
| `testbox generate module <name>` | Scaffold a new TestBox extension module |
| `testbox info` | Show TestBox installation details |
| `testbox reinstall` | Reinstall the CLI-bundled TestBox |
| `testbox docs` | Open TestBox documentation in a browser |
| `testbox apidocs` | Open TestBox API docs in a browser |
