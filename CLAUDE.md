# TestBox Documentation Conventions

This is the GitBook source for the official TestBox documentation, published at
<https://testbox.ortusbooks.com>. Each major version lives on its own branch (`v7.x`,
and so on); the version switcher in GitBook maps to those branches.

## Engine Preference

**BoxLang is the preferred engine.** When a feature exists on both BoxLang and CFML,
BoxLang comes first in every example, tab and list. Write BoxLang as the default voice
of the documentation and treat CFML as the companion, not the other way round.

## Code Examples

### Dual-engine features

When a feature works on **both** BoxLang and CFML, wrap the example in a GitBook tab
group with **BoxLang first, CFML second**:

````markdown
{% tabs %}
{% tab title="BoxLang" %}
{% code title="MyFirstSpec.bx" %}
```java
class extends="testbox.system.BaseSpec"{

    function run(){
        describe( "My First Test", () => {
            it( "can add", () => {
                expect( sum( 1, 2 ) ).toBe( 3 )
            } )
        } )
    }

}
```
{% endcode %}
{% endtab %}

{% tab title="CFML" %}
{% code title="MyTest.cfc" %}
```cfscript
component extends="testbox.system.BaseSpec"{

    function run(){
        describe( "My First Test", function(){
            it( "can add", function(){
                expect( sum( 1, 2 ) ).toBe( 3 );
            } );
        } );
    }

}
```
{% endcode %}
{% endtab %}
{% endtabs %}
````

Where a page separates BDD from xUnit style, the four-tab form is
`BDD - BoxLang`, `xUnit - BoxLang`, `BDD - CFML`, `xUnit - CFML` — again BoxLang first.

**Group by section, not by line.** One tabbed block covering a section's examples reads
far better than a tab group wrapped around every one-line snippet. A tab group whose two
sides differ only by a trailing semicolon is noise; fold those snippets into the nearest
substantive example instead.

### Engine differences to honor

| | BoxLang | CFML |
| --- | --- | --- |
| Fence language | ```` ```java ```` (or ```` ```groovy ```` for xUnit) | ```` ```cfscript ```` |
| File extension | `.bx` | `.cfc` |
| Class keyword | `class` | `component` |
| Statement terminator | no semicolons | semicolons |
| Closures | `() => {}` preferred | `function(){}` |

### BoxLang-only features

When a feature requires BoxLang and has no CFML equivalent, **do not use tabs** — there
is no second side. Mark it with a hint callout immediately after the heading, and write
the examples in BoxLang:

```markdown
{% hint style="info" %}
These matchers require BoxLang. On CFML engines they are guarded and report unsupported
behavior cleanly.
{% endhint %}
```

Say what actually happens on CFML rather than only that the feature is unavailable —
guarded, ignored, or throwing a named exception such as
`TestBox.BoxLangFeatureNotAvailable`.

Set expectations, Range expectations and the Data Navigator matchers are BoxLang-only.

## Callouts

Use GitBook hints, not plain `>` blockquotes:

- `{% hint style="info" %}` — engine requirements, cross-references, context
- `{% hint style="warning" %}` — behavioral changes and upgrade notes
- `{% hint style="success" %}` — tips worth acting on

Behavioral changes carry the version that introduced them, for example
**Changed in TestBox 7.1:**.

## Structure

- Register every new page in `SUMMARY.md`; an unregistered page will not appear in the book.
- Release notes live in `readme/release-history/whats-new-with-<version>.md`, newest first
  in `SUMMARY.md`, with a `description:` frontmatter key holding the release date.
- Add a short paragraph to `readme/release-history/README.md` for each minor release.
- Keep the existing frontmatter on a page you edit, including `metaLinks` and `icon`.

## Before Committing

- Every `SUMMARY.md` target resolves to a file that exists.
- Every relative `.md` link resolves from its own directory.
- New APIs appear in the reference pages people actually browse, not only in the release notes.
