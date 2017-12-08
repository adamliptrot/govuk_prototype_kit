# GOV-DATE-INPUT Documentation

This macro generates an a day month and year `<input>` with `<label>`, and a `<legend>` all in a `<fieldset>`.

## Variables

```
name
id
legend
hint
value
maxYear
errorMessages

```

## Descriptions of variables

Both name and id are used as a pre-fix for generating each input name and id.

Providing the name as 'dob' will generate inputs with labels in the following format:

```
dob.day
dob.month
dob.year
```


| Name          | Description                                                   |
| ------------- |---------------------------------------------------------------|
| name          | sets the base of name of the input                            |
| id            | sets the id of the input, and the for of the label            |
| legend        | sets the legend                          |
| hint          | sets hint text within the legend                              |
| value         | object of {day, month, year}(saved data is automatically set) |
| maxYear       | sets the max year, min is defaulted to 0                      |
| errorMessages | object of validation and related error messages               |



With hint and maxlength if the values are empty, then they are not displayed in the render.

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

```
{% from 'gov-date-input/macro.njk' import govDate with context %}

{{ govDateInput(
  name='dob',
  id='dob',
  legend="Date of birth",
  hint="For example, 31 12 1990",
  maxYear=2020,
  errorMessages = [
    {condition: 'notEmpty', message: "Please enter a date"}
  ]
)}}
```

## Links

- [nunjucks](https://mozilla.github.io/nunjucks/)
- [nunjucks with node](https://mozilla.github.io/nunjucks/getting-started.html)
- [GOVUK elements](https://github.com/alphagov/govuk_elements)
- [GOV.UK frontend toolkit](https://github.com/alphagov/govuk_frontend_toolkit)