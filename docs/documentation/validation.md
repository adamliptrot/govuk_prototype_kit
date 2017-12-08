# Validation

When you have the kit downloaded and set up:

<ol class="list list-number">
<li>`npm install express-validator --save`</li>
<li>Add `var validator = require('express-validator')` and `app.use(validator())` to server.js</li>
<li>Add the catch-all route</li>
<li>Add the macro and layouts folders to the views folder</li>
<li>Extend all your pages from the base.html in the new layouts folder instead of the default layout.html</li>
<li>Add the conditions to the macro call in your view e.g. `errorMessages = [{condition: 'notEmpty', message: "Enter a name"}]`</li>
</ol>

## The validator

This uses the popular [express-validator](https://github.com/ctavan/express-validator) library. It consumes another library which has a decent amount of [pre-built validations](https://github.com/chriso/validator.js).

## Routing

By default the standard routing used in GOV.UK prototypes will be sufficient and you shouldn't need to add custom routing once yo have added the initial set-up route (step 3 above).

This catch-all interrogates the data from the macro-initialised inputs and uses the condition to validate the input data. If the validation fails then the message sent will be sent back to the macro to inform the user of the problem.

## Catch-all route

This should be added to your route.js file.

    router.post('*', function(req, res, next) {
    console.log(req.body)
      if(req.body['errorData']){
        // get the hidden inputs created for each required input
        var errorData = req.body['errorData']
        // if there is only one, make it into an array so we can json parse it
        if(!Array.isArray(errorData)){
          errorData = [errorData]
        }
        // for each of the required inputs perform the check
        for(var re in errorData){
          var checker = JSON.parse(errorData[re])
          if(checker.condition == 'dependsOn'){
            //check to see if the indicated input has been filled
            //see if dependency is radio/checkbox
            for(var i in req.body){
              if(i == checker.dependsOn){
                if(checker.dependsOnValue){
                    // make sure the dependency has the given value
                    if(req.body[i] == checker.dependsOnValue){
                      req.checkBody(checker.input, checker.message).notEmpty()
                    }
                }
              }
            }

          }
          if(checker.condition == 'notEmpty'){
            req.checkBody(checker.input, checker.message).notEmpty()
          }
          if(checker.condition == 'isBoolean'){
            req.checkBody(checker.input, checker.message).isBoolean()
          }
        }
      }

      var errors = req.validationErrors()
      if (errors) {
        var r = req.url
        if(req.url == "/"){r = '/index.html'}
        // pass the errors through to the page
        res.render('./' + r, { errors: errors });
        return;
      } else {
        //standard route from main routes file
        if (req.body['next-page']) {
          res.redirect(req.body['next-page']);
        } else if (req.body){
          for (var propName in req.body) {
            if (req.body.hasOwnProperty(propName) ) {
              eval("req.session." + propName + " = req.body." + propName);
            }}
              next();
        } else {
          next();
        }
      }
    })
