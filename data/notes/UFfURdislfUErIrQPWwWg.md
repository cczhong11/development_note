
# hugo template

基于[go html template](https://pkg.go.dev/text/template)

## variables

```go
{{ .Title }}
{{ $address }}
{{ $address := "123 Main St." }}
```

## functions

```go
{{ FUNCTION ARG1 ARG2 .. }}
{{ add 1 2 }}
```

## include

```go
{{ partial "header.html" . }}
{{ template "_internal/opengraph.html" . }}
```

## logic

```go
// iteration
{{ range $array }}
    {{ . }} <!-- The . represents an element in $array -->
{{ end }}

{{ range $elem_val := $array }}
    {{ $elem_val }}
{{ end }}

{{ range $elem_index, $elem_val := $array }}
   {{ $elem_index }} -- {{ $elem_val }}
{{ end }}

{{ range $array }}
    {{ . }}
{{else}}
    <!-- This is only evaluated if $array is empty -->
{{ end }}
// condition
{{ with .Params.title }}
    <h4>{{ . }}</h4>
{{ end }}

{{ with .Param "description" }}
    {{ . }}
{{ else }}
    {{ .Summary }}
{{ end }}

{{ if (isset .Params "description") }}
    {{ index .Params "description" }}
{{ else }}
    {{ .Summary }}
{{ end }}

{{ if (and (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr")) }}
```

## pipes

```go

{{ (seq 1 5) | shuffle }}
{{ index .Params "disqus_url" | html }}
```

## context

The most easily overlooked concept to understand about Go Templates is that {{ . }} always refers to the current context.

In the top level of your template, this will be the data set made available to it.
Inside of an iteration, however, it will have the value of the current item in the loop; i.e., {{ . }} will no longer refer to the data available to the entire page.


`$` to access global context
```go
<ul>
{{ range .Params.tags }}
  <li>
    <a href="/tags/{{ . | urlize }}">{{ . }}</a>
            - {{ $.Site.Title }}
  </li>
{{ end }}
</ul>
```

## whitespace

```go
<div>
  {{- .Title -}}
</div>

<div>Hello, World!</div>
```