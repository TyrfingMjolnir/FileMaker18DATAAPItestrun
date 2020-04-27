# FileMaker 18 DATA API test run
FileMaker 18 Server test run

Optional: To pretty print JSON install json package from npm
```
brew install nvm
nvm i --lts
nvm use --lts
npm -g i json
```

Check the FileMaker Server version verbosely
```
curl -ksSv -X GET  -H "Content-Type: application/json" https://localhost/fmi/data/v1/productinfo
```

Check the FileMaker Server version pretty printed
```
curl -ksS -X GET  -H "Content-Type: application/json" https://localhost/fmi/data/v1/productinfo | json
```

Pretty printed response
```
{
  "response": {
    "productInfo": {
      "name": "FileMaker Data API Engine",
      "buildDate": "03/16/2020",
      "version": "18.0.4.428",
      "dateFormat": "MM/dd/yyyy",
      "timeFormat": "HH:mm:ss",
      "timeStampFormat": "MM/dd/yyyy HH:mm:ss"
    }
  },
  "messages": [
    {
      "code": "0",
      "message": "OK"
    }
  ]
}
```

Ask for available databases
```
curl -ksS -X GET  -H "Content-Type: application/json" https://localhost/fmi/data/v1/databases | json
```

Pretty printed response
```
{
  "response": {
    "databases": [
      {
        "name": "PushTest"
      }
    ]
  },
  "messages": [
    {
      "code": "0",
      "message": "OK"
    }
  ]
}
```

Generate a login session
```
curl -ksS -X POST -H "Content-Type: application/json" -H "Authorization: Basic cmVzdDpyZXN0" -d "{}" https://localhost/fmi/data/v1/databases/PushTest/sessions | json
```

Response gives you the `token` to use as Bearer in your next calls.
```
{
  "response": {
    "token": "ea83ee0d4473c6b1c85e9595968db70defef006de38015259f05"
  },
  "messages": [
    {
      "code": "0",
      "message": "OK"
    }
  ]
}
```

Ask for available layouts in database of name `PushTest`
```
curl -ksS -X GET  -H "Content-Type: application/json" -H "Authorization: Bearer ea83ee0d4473c6b1c85e9595968db70defef006de38015259f05" -d "{}" https://10.77.96.146/fmi/data/v1/databases/PushTest/layouts | json
```

Available layouts in the hosted file: `PushTest`
```
{
  "response": {
    "layouts": [
      {
        "name": "PushTest"
      },
      {
        "name": "Table"
      }
    ]
  },
  "messages": [
    {
      "code": "0",
      "message": "OK"
    }
  ]
}
```

General form
```
curl -ksS -X GET  -H "Content-Type: application/json" -H "Authorization: Bearer <<paste token here>>" -d "{}" https://localhost/fmi/data/v1/databases/PushTest/layouts/Table/records | json
```

as pr specific example
```
curl -ksS -X GET  -H "Content-Type: application/json" -H "Authorization: Bearer ea83ee0d4473c6b1c85e9595968db70defef006de38015259f05" -d "{}" https://localhost/fmi/data/v1/databases/PushTest/layouts/Table/records | json
```

There is on record in this table that holds the value `test` in the field named `name`
```
{
  "response": {
    "dataInfo": {
      "database": "PushTest",
      "layout": "Table",
      "table": "Table",
      "totalRecordCount": 1,
      "foundCount": 1,
      "returnedCount": 1
    },
    "data": [
      {
        "fieldData": {
          "name": "test"
        },
        "portalData": {},
        "recordId": "1",
        "modId": "0"
      }
    ]
  },
  "messages": [
    {
      "code": "0",
      "message": "OK"
    }
  ]
}
```
