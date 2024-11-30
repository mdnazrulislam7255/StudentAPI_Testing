# StudentAPI Documentation
The Student API is designed to manage and retrieve information about students, including their details, address, and technical details. It supports common operations like creating, getting, updating, deleting student data, and writing test cases . Finally, I have generated a newman report to amalgamate all the test cases.

**Key features:**                                                                                                                                                                         
- Tests for POST, GET, PUT and DELETE requests                                                                                                                                           
- Collection of tests                                                                                                                                                                    
- Environment set up for easy switching between environments
- Pre-request script for data setup
- Test scripts for all assertions and validations

**Technology used**                                                                                                                                                                       
- Postman
- Newman

**Prequisite**                                                                                                                                                                            
- NodeJS
- Postman
- Newman
- Newman HTML report library

**Installation**
  1. Postman (If necessary) [download and install Postman.](https://www.postman.com/downloads/)
  2. Clone the repository

  ``` console
  git clone https://github.com/mdnazrulislam7255/StudentAPI_Testing.git
  ```
  3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
  4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
  5. Newman and Report Installation Process:
     
  - Newman Install Command:
     
   ```console 
      npm install -g newman
   ```
  - Newman Html Report Install Command:
   ```console 
      npm install -g newman-reporter-htmlextra
   ```  

### **Usage**
1. Select Environment:
    -   In Postman, choose the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

# Base URL
The base URL of Student API is: https://thetestingworldapi.com

# Authentication
No authorization is required for this API.

# Endpoints
## Create details of a student

#### Description: Send a request to Create a student.                                                                                                                                     #### Method: POST                                                                                                                                                                        
#### URL: [baseURL](https://thetestingworldapi.com)/api/studentsDetails/                                                                                                                  
#### Headers:                                                                                                                                                                             
 Content-Type: application/json  
#### Pre-request script:
``` console
var ranadomId= pm.variables.replaceIn("{{$randomInt}}")
pm.environment.set("id",ranadomId);

var ranadomfirstName= pm.variables.replaceIn("{{$randomFirstName}}")
pm.environment.set("firstName",ranadomfirstName);
let middleNames = [
    "Michael",
    "Ann",
    "Lee",
    "Marie",
    "James",
    "Grace",
    "David",
    "Rose",
    "John",
    "Eve",
    "Claire",
    "Joseph",
    "Sophia",
    "William",
    "Alexis",
    "Taylor",
    "Rey"
];
let randomMiddleName = middleNames[Math.floor(Math.random() * middleNames.length)];
pm.environment.set("random_middle_name", randomMiddleName);
console.log("Generated Random Middle Name: " + randomMiddleName);

var ranadomlastName= pm.variables.replaceIn("{{$randomLastName}}")
pm.environment.set("lastName",ranadomlastName);

let jsonData = {
    "date_formats": [
        "Tue Aug 13 2024 10:43:26 GMT+0000 (Coordinated Universal Time)",
        "14/02/1997",
        "2024-11-22",
        "12/12/12"
    ]
};

function getRandomDate(start, end) {
    let randomTime = new Date(start.getTime() + Math.random() * (end.getTime() - start.getTime()));
    return randomTime;
}

// Generate a random date within a 30-year range (1990 - 2050)
let startDate = new Date(1900, 0, 1); // Start: Jan 1, 1990
let endDate = new Date(2050, 11, 31); // End: Dec 31, 2050
let randomDate = getRandomDate(startDate, endDate);

// Function to format dates dynamically
function formatRandomDate(date, format) {
    if (format === "DD/MM/YYYY") {
        return `${String(date.getDate()).padStart(2, '0')}/${String(date.getMonth() + 1).padStart(2, '0')}/${date.getFullYear()}`;
    } else if (format === "YYYY-MM-DD") {
        return `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}-${String(date.getDate()).padStart(2, '0')}`;
    } else if (format === "Day of Week Format") {
        return date.toString(); // Full date format like "Tue Aug 13 2024 10:43:26 GMT+0000 (Coordinated Universal Time)"
    } else if (format === "Short Year Format") {
        return `${String(date.getDate()).padStart(2, '0')}/${String(date.getMonth() + 1).padStart(2, '0')}/${String(date.getFullYear()).slice(-2)}`;
    } else {
        throw new Error("Unknown format type");
    }
}

let formats = jsonData.date_formats;
let randomFormat = formats[Math.floor(Math.random() * formats.length)];

// Format the random date
let formattedDate;
if (randomFormat.includes("GMT")) {
    formattedDate = randomDate.toString(); // Handle GMT/UTC format
} else if (randomFormat.includes("/")) {
    formattedDate = formatRandomDate(randomDate, "DD/MM/YYYY");
} else if (randomFormat.includes("-")) {
    formattedDate = formatRandomDate(randomDate, "YYYY-MM-DD");
} else {
    formattedDate = formatRandomDate(randomDate, "Short Year Format");
}
pm.environment.set("random_date", formattedDate);
console.log("Generated Random Date: " + formattedDate);

```
#### Post-response:
```console
var status = pm.response.code;
console.log(status);
switch (status) {
    case 201:
        var jsonData = pm.response.json();

        pm.test("Check Status code 201", function () {
            pm.response.to.have.status(201);
        });
        break;

    case 404:
        pm.test("Something went wrong/Not found");
        break;

    case 500:
        pm.test("Something went wrong");
        break;

    default:
}
var jsonData= pm.response.json();
pm.environment.set("id",jsonData.id);

```
 
#### Request body:                                                                                                                                                                         
``` console
{
 "id":"{{id}}",
 "first_name": "{{firstName}}",
 "middle_name": "{{random_middle_name}}",
 "last_name": "{{lastName}}",
 "date_of_birth": "{{random_date}}"
 }
```
#### Response body:
``` console
{
    "id": 10498839,
    "first_name": "Garry",
    "middle_name": "Claire",
    "last_name": "Strosin",
    "date_of_birth": "1940-10-26"
}
```
## Get student deatils ( all students)
#### Description: Send a request to get all students.   
#### Method: GET                                                                                                                                                                        
#### URL: [baseURL](https://thetestingworldapi.com)/api/studentsDetails/                                                                                                                  
#### Headers:                                                                                                                                                                             
 Content-Type: application/json                                                                                                                                                                                                                                                                                                                             
#### Response body:                                                                                                                                                                            
``` console
[
    {
        "id": 10498832,
        "first_name": "Christa",
        "middle_name": "Rose",
        "last_name": "Streich",
        "date_of_birth": "Tue Jan 29 1935 16:14:28 GMT-0800 (Pacific Standard Time)"
    },
    {
        "id": 10498831,
        "first_name": "Anissa",
        "middle_name": "Joseph",
        "last_name": "Brakus",
        "date_of_birth": "07/02/1928"
    },
    {
        "id": 10498830,
        "first_name": "jordi",
        "middle_name": "black",
        "last_name": "nino",
        "date_of_birth": "10/10/1990"
    },
    {
        "id": 10498829,
        "first_name": "jordi",
        "middle_name": "black",
        "last_name": "nino",
        "date_of_birth": "10/10/1990"
    },
    
,
]
```
## Get a student by ID
#### Description: Send a request to get a student.   
#### Method: GET                                                                                                                                                                        
#### URL: [baseURL](https://thetestingworldapi.com)/api/studentsDetails/{{id}}                                                                                                            
#### Headers:                                                                                                                                                                            
Content-Type: application/json   
#### Post-response script: 
 ``` console
var jsonData = pm.response.json();

pm.test("Validate 'status' is dynamically loaded from response", function () {
    pm.expect(jsonData).to.have.property("status");
    let statusValue = jsonData.status;
    // Handle case where 'status' is a string ("true" or "false")
    if (typeof statusValue === "string") {
        statusValue = (statusValue.toLowerCase() === "true");
    }
    pm.expect(typeof statusValue).to.eql("boolean", `Expected 'status' to be boolean but got '${typeof statusValue}'`);
    pm.expect(statusValue === true || statusValue === false, `Expected 'status' to be true or false, but got '${statusValue}'`).to.be.true;
    console.log("Status value:", statusValue);
});
pm.test("Validate student ID", function () {
    pm.expect(jsonData.data.id).to.equal(pm.environment.get("id"));
});

pm.test("Check First Name", function () {
    pm.expect(jsonData.data.first_name).to.equal(pm.environment.get("firstName"));
});

pm.test("Check Middle Name", function () {
    pm.expect(jsonData.data.middle_name).to.equal(pm.environment.get("random_middle_name"));
});
pm.test("Check Last Name", function () {
    pm.expect(jsonData.data.last_name).to.equal(pm.environment.get("lastName"));
});


let randomDate = pm.environment.get("random_date");
// Define regular expressions for the formats
let formats = [
    /^[A-Za-z]{3} [A-Za-z]{3} \d{2} \d{4} \d{2}:\d{2}:\d{2} GMT[+-]\d{4} \(.*\)$/, // GMT/UTC Format
    /^\d{2}\/\d{2}\/\d{4}$/, // DD/MM/YYYY
    /^\d{4}-\d{2}-\d{2}$/, // YYYY-MM-DD
    /^\d{2}\/\d{2}\/\d{2}$/ // Short Year Format
];

// Check if the random date matches any of the formats
let isValidFormat = formats.some((regex) => regex.test(randomDate));

// Validate the format
pm.test("Random Date Format Validation", function () {
    pm.expect(isValidFormat).to.be.true;
    console.log("Validated Date Format: " + randomDate);
});

pm.test("Validate the data type of the field value", () => {
    pm.expect(jsonData.status).to.be.a("string");
    pm.expect(jsonData.data.id).to.be.a("number");
    pm.expect(jsonData.data.first_name).to.be.a("string");
    pm.expect(jsonData.data.middle_name).to.be.a("string");
    pm.expect(jsonData.data.last_name).to.be.a("string");
    pm.expect(jsonData.data.date_of_birth).to.be.a("string");
});
```

#### Response body: 
```console
{
    "status": "true",
    "data": {
        "id": 10498766,
        "first_name": "Roma",
        "middle_name": "William",
        "last_name": "Wolff",
        "date_of_birth": "1928-03-18"
    }
}
```

## Update a student by ID
#### Description: Send a request to update a student.   
#### Method: PUT                                                                                                                                                                        
#### URL: [baseURL](https://thetestingworldapi.com)/api/studentsDetails/{{id}}                                                                                                            
#### Headers:                                                                                                                                                                            
Content-Type: application/json   
#### Pre-request script: 
``` console
let first_name= pm.variables.replaceIn("{{$randomFirstName}}")
let middleNames = [
    "Michael",
    "Ann",
    "Lee",
    "Marie",
    "James",
    "Grace",
    "David",
    "Rose",
    "John",
    "Eve",
    "Claire",
    "Joseph",
    "Sophia",
    "William",
    "Alexis",
    "Taylor",
    "Rey"
];
let randomMiddleName = middleNames[Math.floor(Math.random() * middleNames.length)];
pm.environment.set("random_middle_name", randomMiddleName);

let last_name= pm.variables.replaceIn("{{$randomLastName}}")
const firstnamePattern = /^[A-Za-z\s]+$/;
const middle_namePattern = /^[A-Za-z\s]+$/;
const last_namePattern = /^[A-Za-z]+$/;

let isFirstNameValid = firstnamePattern.test(first_name);
let isMiddleNameValid =middle_namePattern.test(randomMiddleName);
let isLastNameValid= last_namePattern.test(last_name);
if (!isFirstNameValid || !isMiddleNameValid || !isLastNameValid) {
    pm.environment.set("updateAllowed", "false");
    pm.error("Update failed.");
} else {
    pm.environment.set("updateAllowed", "true");
    pm.environment.set("firstName", first_name);
    pm.environment.set("random_middle_name", randomMiddleName);
    pm.environment.set("lastName",last_name);
}

```
#### Post-response script:
``` console
pm.test("Validate Status Code is 200", function () {
    pm.response.to.have.status(200);
});
pm.test("Update 'status' and 'msg' fields", function () {
    const response = pm.response.json();
    // Normalize and trim spaces from 'msg' field
    const normalizedMsg = response.msg.trim();
    pm.expect(response.status).to.eql("true");
    pm.expect(normalizedMsg).to.eql("update  data success");
});
pm.test("Check if data update is allowed", function () {
    let updateAllowed = pm.environment.get("updateAllowed");
    pm.expect(updateAllowed).to.eql("true", "Data update not allowed due to validation failure.");
});
```
#### Request body:
``` console
{
    "id": {{id}},
    "first_name": "{{firstName}}",
    "middle_name": "{{random_middle_name}}",
    "last_name": "{{lastName}}",
    "date_of_birth": "10/08/1999"
}
```
#### Response body:
``` console
{
    "status": "true",
    "msg": "update  data success"
}
```
## Delete a student by ID
#### Description: Send a request to delete a student.   
#### Method: DELETE                                                                                                                                                                       
#### URL: [baseURL](https://thetestingworldapi.com)/api/studentsDetails/{{id}}                                                                                                            
#### Headers:                                                                                                                                                                            
Content-Type: application/json   
#### Post-response script: 
``` console
let studentId = pm.environment.get("id");

pm.test("Check if studentId is present", function () {
    if (!studentId) {
        console.error("No student ID found.");
        postman.setNextRequest(null); 
    } else {
        pm.expect(studentId).to.be.a('number'); 
        console.log("Student ID: " + studentId);
    }
});

```
#### Response body: 
``` console
{
    "status": "true",
    "msg": "Delete  data success"
}
```

## Create address by ID
#### Description: Send a request to create a student's address.   
#### Method: POST                                                                                                                                                                       
#### URL: [baseURL](https://thetestingworldapi.com)/api/addresses/{{id}}                                                                                                            
#### Headers:                                                                                                                                                                            
Content-Type: application/json   
#### Pre-request script:
``` console
var randomHouseNumber= pm.variables.replaceIn("{{$randomStreetAddress}}")
pm.environment.set("houseNumber",randomHouseNumber);

var randomCity= pm.variables.replaceIn("{{$randomCity}}")
pm.environment.set("city",randomCity);

var randomState= pm.variables.replaceIn("{{$randomCity}}")
pm.environment.set("state",randomState);

var randomCountry= pm.variables.replaceIn("{{$randomCountry}}")
pm.environment.set("Country",randomCountry);

var randomCountryCode= pm.variables.replaceIn("{{$randomCountryCode}}")
pm.environment.set("CountryCode",randomCountryCode);

var randomPhoneNumber= pm.variables.replaceIn("{{$randomPhoneNumber}}")
pm.environment.set("home",randomPhoneNumber);

var randomPhoneNumber= pm.variables.replaceIn("{{$randomPhoneNumber}}")
pm.environment.set("mobile",randomPhoneNumber);

var randomCountryCode1= pm.variables.replaceIn("{{$randomCountryCode}}")
pm.environment.set("CountryCode1",randomCountryCode1);

var randomPhoneNumber1= pm.variables.replaceIn("{{$randomPhoneNumber}}")
pm.environment.set("home1",randomPhoneNumber1);

var randomPhoneNumber1= pm.variables.replaceIn("{{$randomPhoneNumber}}")
pm.environment.set("mobile1",randomPhoneNumber1);
```
#### Post-response script:
```console
pm.test("Validate Status Code is 200", function () {
    pm.response.to.have.status(200);
});
pm.test("Validate 'status' and 'msg' fields", function () {
    const response = pm.response.json();
    const normalizedMsg = response.msg.trim();
    pm.expect(response.status).to.eql("true");
    pm.expect(normalizedMsg).to.eql("Add  data success");
});
```
#### Request body:
``` console
{
    "Permanent_Address": {
        "House_Number": "{{houseNumber}}",
        "City": "{{city}}",
        "State": "{{state}}",
        "Country": "{{Country}}",
        "PhoneNumber": [
            {
                "Std_Code": "{{CountryCode}}",
                "Home": "{{home}}",
                "Mobile": "{{mobile}}"
            },
             {
                "Std_Code": "{{CountryCode1}}",
                "Home": "{{home1}}",
                "Mobile": "{{mobile1}}"
            }
        ]
    },
    "stId": {{id}}
}
```
#### Response body:
``` console
{
    "status": "true",
    "msg": "Add  data success"
}
```
## Get address by ID
#### Description: Send a request to get a student's address.   
#### Method: GET                                                                                                                                                                       
#### URL: [baseURL](https://thetestingworldapi.com)/api/addresses/{{id}}                                                                                                            
#### Headers:                                                                                                                                                                            
Content-Type: application/json   
#### Response body:
``` console
[
    {
        "Permanent_Address": {
            "House_Number": "3135 Fadel Squares",
            "City": "Loraineburgh",
            "State": "Kochland",
            "Country": "Lesotho",
            "PhoneNumber": [
                {
                    "Std_Code": "MP",
                    "Home": "770-784-8656",
                    "Mobile": "244-843-1639"
                },
                {
                    "Std_Code": "PM",
                    "Home": "545-388-7968",
                    "Mobile": "344-250-6065"
                }
            ]
        },
        "Current_Address": null,
        "stId": "10498766"
    }
]
```
## Update address by ID
#### Description: Send a request to update a student's address.   
#### Method: PUT                                                                                                                                                                       
#### URL: [baseURL](https://thetestingworldapi.com)/api/addresses/{{id}}                                                                                                            
#### Headers:                                                                                                                                                                            
Content-Type: application/json   
#### Request body:
``` console
{
    "Permanent_Address": {
        "House_Number": "191/B",
        "City": "{{city}}",
        "State": "{{state}}",
        "Country": "{{Country}}",
        "PhoneNumber": [
            {
                "Std_Code": "{{CountryCode}}",
                "Home": "{{home}}",
                "Mobile": "{{mobile}}"
            },
            {
                "Std_Code": "{{CountryCode1}}",
                "Home": "{{home1}}",
                "Mobile": "{{mobile1}}"
            }
        ]
    },
    "stId": {{id}}
}
```
#### Response body
``` console
{
    "status": "true",
    "msg": "Update  data success"
}
```
## Delete address by ID
#### Description: Send a request to delete a student's address.   
#### Method: DELETE                                                                                                                                                                       
#### URL: [baseURL](https://thetestingworldapi.com)/api/addresses/{{id}} 
#### Post response script:
``` console
let s_address = pm.environment.get("id");

pm.test("Check if student address is present", function () {
    if (!s_address) {
        console.error("No address is found.");
        postman.setNextRequest(null); 
    } else {
        pm.expect(s_address).to.be.a('number'); 
        console.log("Student ID: " + s_address);
    }
});
```
#### Response body:
``` console
{
    "status": "true",
    "msg": "Delete  data success"
}
```
## Create technical skills by ID
#### Description: Send a request to create a student's technical skills.   
#### Method: POST                                                                                                                                                                       
#### URL: [baseURL](https://thetestingworldapi.com)/api/technicalskills/{{id}}                                                                                                            
#### Headers:                                                                                                                                                                            
Content-Type: application/json   
#### Pre-request script:
``` console
let languages = [
   "English",
    "Mandarin Chinese",
    "Hindi",
    "Spanish",
    "French",
    "Arabic",
    "Bengali",
    "Russian",
    "Portuguese",
    "Japanese",
    "Punjabi",
    "German",
    "Korean",
    "Italian",
    "Turkish",
    "Tamil",
    "Urdu",
    "Vietnamese",
    "Persian",
    "Swahili",
    "Malay",
    "Thai",
    "Dutch",
    "Greek",
    "Hebrew",
    "Polish",
    "Czech",
    "Swedish",
    "Hungarian",
    "Zulu",
    "Amharic"
];

let randomLangauge = languages[Math.floor(Math.random() * languages.length)];
pm.environment.set("random_language", randomLangauge);
console.log("Generated Random Middle Name: " + randomLangauge);

var yearOfExp= pm.variables.replaceIn("{{$randomInt}}")
pm.environment.set("experience", yearOfExp);

let jsonData = {
    "date_formats": [
        "Tue Aug 13 2024 10:43:26 GMT+0000 (Coordinated Universal Time)",
        "14/02/1997",
        "2024-11-22",
        "12/12/12"
    ]
};

// Function to generate a random date between a range
function getRandomDate(start, end) {
    let randomTime = new Date(start.getTime() + Math.random() * (end.getTime() - start.getTime()));
    return randomTime;
}

// Generate a random date within a 30-year range (1990 - 2050)
let startDate = new Date(1900, 0, 1); // Start: Jan 1, 1990
let endDate = new Date(2050, 11, 31); // End: Dec 31, 2050
let randomDate = getRandomDate(startDate, endDate);

// Function to format dates dynamically
function formatRandomDate(date, format) {
    if (format === "DD/MM/YYYY") {
        return `${String(date.getDate()).padStart(2, '0')}/${String(date.getMonth() + 1).padStart(2, '0')}/${date.getFullYear()}`;
    } else if (format === "YYYY-MM-DD") {
        return `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(2, '0')}-${String(date.getDate()).padStart(2, '0')}`;
    } else if (format === "Day of Week Format") {
        return date.toString(); // Full date format like "Tue Aug 13 2024 10:43:26 GMT+0000 (Coordinated Universal Time)"
    } else if (format === "Short Year Format") {
        return `${String(date.getDate()).padStart(2, '0')}/${String(date.getMonth() + 1).padStart(2, '0')}/${String(date.getFullYear()).slice(-2)}`;
    } else {
        throw new Error("Unknown format type");
    }
}

// Randomly pick a format from JSON data
let formats = jsonData.date_formats;
let randomFormat = formats[Math.floor(Math.random() * formats.length)];

// Format the random date
let formattedDate;
if (randomFormat.includes("GMT")) {
    formattedDate = randomDate.toString(); // Handle GMT/UTC format
} else if (randomFormat.includes("/")) {
    formattedDate = formatRandomDate(randomDate, "DD/MM/YYYY");
} else if (randomFormat.includes("-")) {
    formattedDate = formatRandomDate(randomDate, "YYYY-MM-DD");
} else {
    formattedDate = formatRandomDate(randomDate, "Short Year Format");
}
// Save the random date as an environment variable
pm.environment.set("lastused", formattedDate);
console.log("Generated Random Date: " + formattedDate);
```
#### Post response script:
``` console
pm.test("Validate Status Code is 200", function () {
    pm.response.to.have.status(200);
});
const body = pm.request.body.raw ? JSON.parse(pm.request.body.raw) : {};

pm.test("Validate Required Fields in Request Body", function () {
    pm.expect(body).to.have.property("id").that.is.a("number"); 
    pm.expect(body).to.have.property("language").that.is.an("array");
    pm.expect(body).to.have.property("yearexp").that.is.a("string");
    pm.expect(body).to.have.property("lastused").that.is.a("string");
    pm.expect(body).to.have.property("st_id").that.is.a("number"); 
});
```
#### Request body:
``` console
{
    "id": 1,
    "language": [
        "English",
        "{{random_language}}"
    ],
    "yearexp": "{{experience}}",
    "lastused": "{{lastused}}",
    "st_id": {{id}}
}
```
#### Response body:
``` console
{
    "status": "true",
    "msg": "Add  data success"
}
```
## Final details of student by ID
#### Description: Send a request to get  final details of student by ID.   
#### Method: GET                                                                                                                                                                       
#### URL: [baseURL](https://thetestingworldapi.com)/api/FinalStudentDetails/{{id}}                                                                                                        
#### Headers:                                                                                                                                                                            
Content-Type: application/json   
#### Post response script:
``` console

var status = pm.response.code;
console.log(status);
switch (status) {
    case 200:
        var jsonData = pm.response.json();

        pm.test("Check Status code 200 OK", function () {
            pm.response.to.have.status(200);
        });
        pm.test("Validate 'status' is dynamically loaded from response", function () {
            pm.expect(jsonData).to.have.property("status");
            let statusValue = jsonData.status;
            // Handle case where 'status' is a string ("true" or "false")
            if (typeof statusValue === "string") {
                statusValue = (statusValue.toLowerCase() === "true");
            }
            pm.expect(typeof statusValue).to.eql("boolean", `Expected 'status' to be boolean but got '${typeof statusValue}'`);
            pm.expect(statusValue === true || statusValue === false, `Expected 'status' to be true or false, but got '${statusValue}'`).to.be.true;
            console.log("Status value:", statusValue);
        });

        pm.test("Validate First Name", function () {
            var jsonData = pm.response.json();
            pm.expect(jsonData.data.first_name).to.equal(pm.environment.get("firstName"));
        });

        pm.test("Validate Middle Name", function () {
            pm.expect(jsonData.data.middle_name).to.equal(pm.environment.get("random_middle_name"));
        });
        pm.test("Validate Last Name", function () {
            pm.expect(jsonData.data.last_name).to.equal(pm.environment.get("lastName"));
        });

        let randomDate = pm.environment.get("random_date");
        // Define regular expressions for the formats
        let formats = [
            /^[A-Za-z]{3} [A-Za-z]{3} \d{2} \d{4} \d{2}:\d{2}:\d{2} GMT[+-]\d{4} \(.*\)$/, // GMT/UTC Format
            /^\d{2}\/\d{2}\/\d{4}$/, // DD/MM/YYYY
            /^\d{4}-\d{2}-\d{2}$/, // YYYY-MM-DD
            /^\d{2}\/\d{2}\/\d{2}$/ // Short Year Format
        ];

        // Check if the random date matches any of the formats
        let isValidFormat = formats.some((regex) => regex.test(randomDate));

        // Validate the format
        pm.test("Random Date Format Validation", function () {
            pm.expect(isValidFormat).to.be.true;
            console.log("Validated Date Format: " + randomDate);
        });

        pm.test("Validate language", function () {
            for (i = 0; i < jsonData.data.TechnicalDetails.length; i++) {
                if (pm.expect(jsonData.data.TechnicalDetails[i].language == pm.environment.get("random_language"))) {
                    pm.test("Language Validation is successful");

                }
                else {
                    pm.expect(pm.response.code).to.eql(404);
                }
            }

        });
        pm.test("Validate year of experience", function () {
            for (i = 0; i < jsonData.data.TechnicalDetails.length; i++) {
                if (pm.expect(jsonData.data.TechnicalDetails[i].yearexp == pm.environment.get("experience"))) {
                    pm.test("Year of experience Validation is successful");
                }
                else {
                    pm.expect(pm.response.code).to.eql(404);
                }
            }
        });
        pm.test("Validate date", function () {
            for (i = 0; i < jsonData.data.TechnicalDetails.length; i++) {
                if (pm.expect(jsonData.data.TechnicalDetails[i].lastused == pm.environment.get("lastused"))) {
                    pm.test("Validation of lastused is successful")
                }
                else {
                    pm.expect(pm.response.code).to.eql(404);
                }
            }
        });
        pm.test("Validate permanent address", function () {
            for (i = 0; i < jsonData.data.Address.length; i++) {
                if (pm.expect(jsonData.data.Address[i].Permanent_Address.House_Number == pm.environment.get("houseNumber"))) {
                    pm.test("House_Number Validation is successful")
                }
                if (pm.expect(jsonData.data.Address[i].Permanent_Address.City == pm.environment.get("city"))) {
                    pm.test("City Validation is successful")
                }
                if (pm.expect(jsonData.data.Address[i].Permanent_Address.State == pm.environment.get("state"))) {
                    pm.test("StateValidation is successful")
                }
                if (pm.expect(jsonData.data.Address[i].Permanent_Address.Country == pm.environment.get("Country"))) {
                    pm.test("Country Validation is successful")
                }
            }
        });
        pm.test("Phone Number Validation", function () {
            let jsonData = pm.response.json();

            for (let i = 0; i < jsonData.data.Address.length; i++) {
                let permanentAddress = jsonData.data.Address[i].Permanent_Address;

                if (permanentAddress && permanentAddress.PhoneNumber) {
                    for (let j = 0; j < permanentAddress.PhoneNumber.length; j++) {
                        let phoneNumber = permanentAddress.PhoneNumber[j];
                        if (pm.expect(phoneNumber.Std_Code == pm.environment.get("CountryCode"))) {
                            pm.test("Country Code Validation is successful")
                        }
                        if (pm.expect(phoneNumber.Home == pm.environment.get("home"))) {
                            pm.test("Home number validation is successful")
                        }
                        if (pm.expect(phoneNumber.Mobile == pm.environment.get("mobile"))) {
                            pm.test("Mobile number validation is successful")
                        }
                        if (pm.expect(phoneNumber.Std_Code == pm.environment.get("CountryCode1"))) {
                            pm.test("Country Code 2 Validation is successful")
                        }
                        if (pm.expect(phoneNumber.Home == pm.environment.get("home1"))) {
                            pm.test("Home number  2 validation is successful")
                        }
                        if (pm.expect(phoneNumber.Mobile == pm.environment.get("mobile1"))) {
                            pm.test("Mobile number 2 validation is successful")
                        }
                    }
                }
                else {
                    pm.expect.fail(`Permanent_Address or PhoneNumber is missing at Address[${i}]`);
                }
            }
        });
        pm.test("Validate current address", function () {
            pm.expect(jsonData.data.Address[0].Current_Address).to.eql(null);
        });

        break;

    case 404:
        pm.test("Something went wrong/Not found");
        break;

    case 500:
        pm.test("Something went wrong");
        break;

    default:
}

pm.test("Content-Type is present", function () {
    pm.expect(pm.response.headers.get('Content-Type')).to.eql('application/json; charset=utf-8')
});

pm.test("Check if status is a string", function () {
    pm.expect(jsonData.status).to.be.a('string');
});

pm.test("Check if first_name is a string", function () {
    pm.expect(jsonData.data.first_name).to.be.a('string');
});

pm.test("Check if middle_name is a string", function () {
    pm.expect(jsonData.data.middle_name).to.be.a('string');
});

pm.test("Check if last_name is a string", function () {
    pm.expect(jsonData.data.last_name).to.be.a('string');
});

pm.test("Check if date_of_birth is a string", function () {
    pm.expect(jsonData.data.date_of_birth).to.be.a('string');
});

pm.test("Check if TechnicalDetails is an array", function () {
    pm.expect(jsonData.data.TechnicalDetails).to.be.an('array');
});

pm.test("Check if each TechnicalDetail has a valid id (number)", function () {
    for (let i = 0; i < jsonData.data.TechnicalDetails.length; i++) {
        pm.expect(jsonData.data.TechnicalDetails[i].id).to.be.a('number');
    }
});

pm.test("Check if language is an array of strings", function () {
    for (let i = 0; i < jsonData.data.TechnicalDetails.length; i++) {
        pm.expect(jsonData.data.TechnicalDetails[i].language).to.be.an('array');
        jsonData.data.TechnicalDetails[i].language.forEach(function (language) {
            pm.expect(language).to.be.a('string');
        });
    }
});

pm.test("Check if yearexp is a string", function () {
    for (let i = 0; i < jsonData.data.TechnicalDetails.length; i++) {
        pm.expect(jsonData.data.TechnicalDetails[i].yearexp).to.be.a('string');
    }
});

pm.test("Check if lastused is a string", function () {
    for (let i = 0; i < jsonData.data.TechnicalDetails.length; i++) {
        pm.expect(jsonData.data.TechnicalDetails[i].lastused).to.be.a('string');
    }
});

pm.test("Check if Address is an array", function () {
    pm.expect(jsonData.data.Address).to.be.an('array');
});

pm.test("Check if Permanent_Address is an object", function () {
    for (let i = 0; i < jsonData.data.Address.length; i++) {
        pm.expect(jsonData.data.Address[i].Permanent_Address).to.be.an('object');
    }
});

pm.test("Check if PhoneNumber is an array of objects", function () {
    for (let i = 0; i < jsonData.data.Address.length; i++) {
        pm.expect(jsonData.data.Address[i].Permanent_Address.PhoneNumber).to.be.an('array');
        jsonData.data.Address[i].Permanent_Address.PhoneNumber.forEach(function (phone) {
            pm.expect(phone).to.be.an('object');
            pm.expect(phone.Std_Code).to.be.a('string');
            pm.expect(phone.Home).to.be.a('string');
            pm.expect(phone.Mobile).to.be.a('string');
        });
    }
});

pm.test("Check if stId is a string", function () {
    for (let i = 0; i < jsonData.data.Address.length; i++) {
        pm.expect(jsonData.data.Address[i].stId).to.be.a('string');
    }
});
```
#### Response body:
``` console
{
    "status": "true",
    "data": {
        "first_name": "Garry",
        "middle_name": "Claire",
        "last_name": "Strosin",
        "date_of_birth": "1940-10-26",
        "TechnicalDetails": [
            {
                "id": 800229,
                "language": [
                    "English",
                    "Hebrew"
                ],
                "yearexp": "21",
                "lastused": "10/10/1944",
                "st_id": "10498839"
            }
        ],
        "Address": [
            {
                "Permanent_Address": {
                    "House_Number": "890 Schuppe Pine",
                    "City": "North Brooklyn",
                    "State": "Dallinfurt",
                    "Country": "Western Sahara",
                    "PhoneNumber": [
                        {
                            "Std_Code": "DJ",
                            "Home": "595-810-5674",
                            "Mobile": "880-364-8974"
                        },
                        {
                            "Std_Code": "KE",
                            "Home": "862-562-8795",
                            "Mobile": "696-958-9557"
                        }
                    ]
                },
                "Current_Address": null,
                "stId": "10498839"
            }
        ]
    }
}
```
## Run Command:  
- Run Command for Console: 
```console 
newman run StudentAPI.postman_collection.json -e student_env.postman_environment.json 
```
- Run Command for Report: 
```console 
newman run StudentAPI.postman_collection.json -e student_env.postman_environment.json -r cli,htmlextra
```

# Newman report
![image](https://github.com/user-attachments/assets/b684df05-172a-463d-bf23-044914686655)
![image](https://github.com/user-attachments/assets/88a0826b-8249-4231-81db-37b1cf2d192f)
![image](https://github.com/user-attachments/assets/e17dc54c-fe5a-440a-a292-80ffa11e568d)





