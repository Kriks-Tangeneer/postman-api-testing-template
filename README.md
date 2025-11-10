
# Postman: API Testing Template

This template provides a standardized structure for implementing API tests, ensuring consistency across
microservices and enabling easier onboarding, maintenance, and automation.


## Usage Guideline

- Use this pseudocode as a baseline for writing new Postman tests or scripting in your test automation framework.
- Replace "items" , "status" , and other hardcoded fields with endpoint-specific data.
- Maintain consistent naming in test titles and logs for easier filtering in reports.

## Template

```javascript
// ===========================================
// Request: [Method] [URL]
// Purpose: [Description]
// Auth:    [Auth Type] [Auth Details]
// Expected status: [Response code] [Response Status]
// ===========================================

// 🧩 ~~ Test Configuration ~~ 🧩
var runBaselineTests = pm.collectionVariables.get("runBaselineTests");              // Set to false to skip baseline tests
var runFieldDefintionTests = pm.collectionVariables.get("runFieldDefintionTests");  // Set to false to skip field definition tests
var runDatatypeTests = pm.collectionVariables.get("runDatatypeTests");              // Set to false to skip datatype tests
var runFunctionalTests = pm.collectionVariables.get("runFunctionalTests");          // Set to false to skip datatype tests 

// 🧩 ~~ Generic Uptime Tests ~~ 🧩
pm.test("[Service Name] Request Name - Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("[Service Name] Request Name - Response status is OK", function () {
    pm.expect(pm.response.status).to.eql("OK");
});

// Conditional check for further testing
if (pm.response.code === 200) 
{
    // 🧩 ~~ Variable Declarations ~~ 🧩
    var jsonData = pm.response.json();
  
    if(runBaselineTests)
    {
        // 🧩 ~~ Field Definition Tests ~~ 🧩
        if(runFieldDefintionTests)
        {
            pm.test("[Service] Request Name - Response contains'success' field", function () {
                pm.expect(jsonData).to.have.property("success");
            });
        }

        // 🧩 ~~ Datatype Tests ~~ 🧩
        if (runDatatypeTests)
        {
            pm.test("[Service] Request Name - 'success' is a boolean", function () {
                pm.expect(typeof (jsonData.success)).to.eq("boolean");
            });
        }

        // 🧩 ~~ Functional Tests ~~ 🧩
        if (runFunctionalTests)
        {
            pm.test("[Service] Request Name - 'success' is true", function () {
                pm.expect(jsonData.success).to.be.true;
            });
        }
    }
    
    console.log(pm.info.requestName + " : PASS");
} 
else 
{
    console.log(pm.info.requestName + " : FAIL");
    console.log(pm.response.text());//log response body to console if request fails to aid debugging
}
```


## Author

- [@Kriks-Tangeneer](https://github.com/Kriks-Tangeneer)

