# GOV-RADIOS Documentation

This macro generates a selection of `<input>` with `<label>` based on what you supply, and a `<legend>` all in a `<fieldset>`.

## Variables

```
name
id
value
label
isSelected
dataTarget

```

## Descriptions of variables

| Name          | Description                                                   |
| ------------- |---------------------------------------------------------------|
| name          | sets the name for the input                                   |
| id            | sets the id for the input                                     |
| value         | sets the value for the input                                  |
| label         | sets the label                                                |
| isSelected    | boolean to check the input                                    |
| dataTarget    | the id of another input for use with show/hide script         |


## Using with express

You will need to expose the views to the nunjucks config, an example is below.

```javascript

const appViews = [path.join(__dirname, '/app/views/'),
                  path.join(__dirname, '/node_modules/govuk-elements-nunjucks/components/'),
                  path.join(__dirname, '/lib/')]

const nunjucksAppEnv = nunjucks.configure(appViews, {
  autoescape: true,
  express: app,
  noCache: true,
  watch: true
})
```

## Example in use
Include the nunjucks macro and render it like so:

```javascript
{% from 'gov-checkbox-single/macro.njk' import govCheckboxSingle with context %}

{{ govCheckboxSingle(
  id = "declaration", 
  name = "declaration", 
  value = "agreed", 
  label = "I agree to the conditions"
) }}

```

## Links

- [nunjucks](https://mozilla.github.io/nunjucks/)
- [nunjucks with node](https://mozilla.github.io/nunjucks/getting-started.html)
- [GOVUK elements](https://github.com/alphagov/govuk_elements)
- [GOV.UK frontend toolkit](https://github.com/alphagov/govuk_frontend_toolkit)