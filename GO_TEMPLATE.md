# Go Template Examples

## Introduction

This README provides examples of Helm template functionalities using Go template syntax. These examples cover various scenarios such as defining functions, conditionals, using predefined functions, working with blocks of data, loops, variables, and more.

## Define Function

```yaml
{{- define "helm.name" -}}
{{- default .Chart.Name .Values.nameOverride | trunc 63 | trimSuffix "-" }}
{{- end }}
```

## Call a Function

```yaml
{{ include "helm.fullname" . }}
```

## Conditional Statements

### If Statement

```yaml
{{- if .Values.fullnameOverride }}
{{- .Values.fullnameOverride | trunc 63 | trimSuffix "-" }}
{{- else if .Values.fullnameOverride }}
{{- else }}
{{- end }}
```

### Nested If Statement

```yaml
{{- if not .Values.autoscaling.enabled }}

{{- if and .Values.autoscaling.enabled .Values.autoscaling.enabled }}
```

## Using Function Output

```yaml
{{- toYaml .Values.podSecurityContext | nindent 8 }}
```

## Using Values

```yaml
use values
{{ .Values.service.port }}
```

## Predefined Functions

- `nindent 4`: Put its input on a new line with 4 spaces before it.
- `indent 4`: Put 4 spaces before its input.
- `quote`: Put its input in double quotes.
- `default "txt"`: If input is empty, output is "txt"; otherwise, output is input.
- `upper`: Uppercase its input.

## With Statement

### Example 1

```yaml
// values.yaml
customblock:
  author:
    name:
      - parham
      - roya
      - puria
// action
{{- with .Values.customblock.author.name }}
  author:
  {{- toYaml . | nindent 2 }}
{{- else }} // if .Values.customblock.author.name is empty
{{"this block is empty" | nindent 2 }}
{{- end }}
// output
author:
  - parham
  - roya
  - puria
```

### Example 2

```yaml
// values.yaml
customblock:
  author:
    name:
      - parham
      - roya
      - puria
// action
{{- range .Values.customblock.author.name }}
  author:
  - {{ . }}
{{- end }}
// output
author:
  - parham
author:
  - roya
author:
  - puria
```

## Loop with Map

### Example

```yaml
// values.yaml
image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: ""
// action
image:
{{- range $key,$value := .Values.image }}
  {{$key}}: {{$value | quote}}
{{- end }}
// output
image:
  repository: "nginx"
  pullPolicy: "IfNotPresent"
  tag: ""
```

## Variable Declaration

```yaml
{{ $myVar := true }}
{{- if $myVar }}
{{ "it's true" }}
{{- end }}
```

Feel free to use and modify these examples as needed for your Helm charts.