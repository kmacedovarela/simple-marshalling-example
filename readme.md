Marshalling example
=======================

# How to test:

1. Import this project to your business central using for example
2. Deploy the project in kie server
3. Test the following requests using a user with *kie-server* and *rest-all* roles. 

## Create a process 

POST http://localhost:8080/kie-server/services/rest/server/containers/marshalling-sample/processes/myprocess/instances with JSON:
{ 
	"person" : { 
		"com.myspace.Person" : {
			"name" : "John Doe",
			"age" : 19,
			"sex": "female"
		}
	},
	"randomNumber": 59
}

You can also use this command (replace the authorization token with a valid token on your environment (you can generate one here [here](https://www.blitter.se/utils/basic-authentication-header-generator/) if you need): 
````
curl -X POST \
  'http://localhost:8080/kie-server/services/rest/server/containers/marshalling-sample/processes/myprocess/instances?person=' \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic cmhwYW1BZG1pbjpyZWRoYXRAMTIz' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: a1b4bce9-c730-4b04-af6d-d26bc6d55e32' \
  -H 'cache-control: no-cache' \
  -d '{ 
	"person" : { 
		"com.myspace.Person" : {
			"name" : "John Doe",
			"age" : 19,
			"sex": "male"
		}
	},
	"randomNumber": 59
}'
````

The response will be the ID of the new process instance created. The process instance will stay active, waiting on the user task "Task for male". In this situation, you are able to collect the information for example from the task or from the process instance. 

=== Retrieving the variable values from the process instance

The process created has an ID which you can use to retrieve the variables by requesting /server/containers/marshalling-sample/processes/instances/[[PROCESS_ID]]/variables

Example considering a process with ID 1: GET http://localhost:8080/kie-server/services/rest/server/containers/marshalling-sample/processes/instances/1/variables

The result is: 

````bash
curl -X GET 'http://localhost:8080/kie-server/services/rest/server/containers/marshalling-sample/processes/instances/1/variables' -H 'Accept: application/json'  -H 'Authorization: Basic a2FyaW5hOmFzZGFzZGFzZA=='
{
  "person" : {
  "com.myspace.Person" : {
    "sex" : "male",
    "age" : 19,
    "name" : "John Doe"
  }
},
  "initiator" : "karina",
  "randomNumber" : 59
}
```
