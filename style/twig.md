Table of Contents
=================

* [Syntax](#syntax)
* [File names](#file-names)
* [Indentation](#indentation)
* [Comments](#comments)
* [Long hashes or argument lists](#long-hashes-or-argument-lists)
* [Filters](#filters)
* [Macros](#macros)
* [General style rules](#general-style-rules)

## Twig Style Guide

The Twig template engine, and it's other language siblings, are used throughout SNworks products. This guide is based on the original [Twig style guide](http://twig.sensiolabs.org/doc/coding_standards.html)

- TODO(mikejoseph): Add bits about fetch tags
- TODO(mikejoseph): Linters, yo.

### Syntax

```twig
{% set variableNameLikeThis %}
{% set CONSTANT_LIKE_THIS %}
```

While Twig doesn't support actual constants, using underscored uppercase makes it clear that you shouldn't change this variable.

### File names

```
file-name-has-hyphens.tpl
```

Don't use spaces or underscores in file names.

### Indentation

Use 4-space indentation, *do not use tabs*. Like, ever.

Extra indentation should be used to clearly distinguish multiline conditionals from the following block of code (similar to the PEP8 rule for Python code).

Bad:
```twig
{% if foo.bar == 'baz' and flip == true and
fork == 'bar' %}
    something
{% endif %}
```

Also bad:
```twig
{% if foo.bar == 'baz' and flip == true and
    fork == 'bar' %}
    something
{% endif %}
```

Good:
```twig
{% if foo.bar == 'baz' and flip == true and
        fork == 'bar' %}
    something
{% endif %}
```

Indent one level for each code block, including HTML:

```twig
{% block content %}
    <div class="container">
        {% if foo %}
            Hello {{ name }}
        {% endif %}
    </div>
{% endblock %}
```

### Comments

Inline comments should start with a comment marker and a space:

```twig
{% set foo = 'bar' %} {# foo is used to make fraz #}
```

Block comments should start with a start marker on its own line, and end with a end marker on its own line. All text should be indented within the block:

```twig
{#
    This is a comment

    It runs a few lines
#}
```

### Long hashes or argument lists

Liberal use of new lines and indentation makes larger, more complex calls much more readable. When including arrays and hashes, always open the array or hash on a new line, and close the statement on its own line:

Bad:
```twig
{{layoutRender.mediaBlockGrid(recent, {"columns":cols, "title":true, "rowClass":"media-grid gallery-grid", "headlineSize": 4})}}
```

Good:
```twig
{{layoutRender.mediaBlockGrid(
    recent,
    {
        "columns": cols,
        "title": true,
        "rowClass": "media-grid gallery-grid",
        "headlineSize": 4
    }
)}}
```

### Filters

Filters should have no spaces between the pipe and filter name. Additionally, there should be no space between filter name and opening parenthesis. Arguments lists should have a single space after the comma and no spaces between the open and closing parenthesis and any arguments:

```twig
{{ foo|url() }}

{{ "now"|date('Y-m-d') }}
```

### Macros

Macros are any code that can be used in multiple places. **A macro that is only used once is not a macro.**

Macro names should be camelCased with no spaces between the name and the open parenthesis. Arguments lists should have a single space after the comma and no spaces between the open and closing parenthesis and any arguments:

```twig
{% macro thisIsMySweetMacro() %}
    do some stuff
{% endmacro %}

{% macro thisMacroHasArguments(foo, bar) %}
    do some stuff
{% endmacro %}
```

Inline macros are strongly encouraged and should be namespaced with `_my`:

```twig
{% macro showLine(article) %}
    <h1><a href="{{ article.url }}">{{ article.headline }}</a></h1>
{% endmacro}

{% import self as _my %}

{% for article in articles %}
    {{ _my.showLine(article) }}
{% endfor %}
```

### General style rules

Put one (and only one) space after the start of a delimiter ({{, {%, and {#) and before the end of a delimiter (}}, %}, and #}):

```twig
{{ fork }}

{% if foo %}
    ...
{% endif %}

{% for foo in bar %}
    ...
{% endfor %}
```

Put one (and only one) space after the : sign in hashes and , in arrays and hashes:

```twig
{% set foo = {'foo': 'bar'} %}

{% set bar = [1, 2, 3] %}
```

Do not put any spaces after an opening parenthesis and before a closing parenthesis in expressions:

```twig:
{{ val + (bar * 3)}}
```

Do not put any spaces before and after the following operators: |, ., .., []:

```twig:
{{ foo|upper|lower }}
{{ user.name }}
{{ user[0] }}
{% for i in 1..12 %}{% endfor %}
```

Avoid key notation with braces wherever possible:

Bad:
```twig
{{ article[headline] }}
```

Good:
```twig
{{ article.headline }}
```

There are some instances where it is necessary, though, like looping through properites or generating a key name:

```tiwg
{% set key = 'text' + time %}
{{ article[key] }}

{% for prop,val in articleKeys %}
    {{ article[prop] }}
{% endfor %}
```