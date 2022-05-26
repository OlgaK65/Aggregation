aggregation - maven java project,
swagger documented (available at /swagger-ui/index.html)

Application with 2 APIs:

1. aggregation:
/API/aggregate
    put:
       description: Creates a session with exactly two samples
       request:
          body:
           app/json:
               {"machineId": {"type":"string"},
                "epId":{"type":"string"},
                "timestamp": {"type":"number"},
                "measurementValue": {"type":"number"}
}
       response:
          null if there is no object with this mashineId in the database,
                  or there is an object with the same mashineId and epId
          or
            body:
		{
			"a": {
					"machineId": {"type":"string"},
					"epId": {"type":"string"},
					"timestamp": {"type":"number"},
					"measurementValue": {"type":"number"}
				  },
			"b": {
					"machineId": {"type":"string"},
					"epId": {"type":"string"},
					"timestamp": {"type":"number"},
					"measurementValue": {"type":"number"}
				  }
		},

		if there is an object in the database with this mashineId, but with a different epId

		status: 200

2. delayed aggregation:
/api/aggregate_with_lag
    put:
       description: Creates a session with exactly two samples
       request:
          body:
           app/json:
               {"machineId": {"type":"string"},
                "epId":{"type":"string"},
                "timestamp": {"type":"number"},
                "measurementValue": {"type":"number"}
}
       response:
          null if there is no object with this mashineId in the database,
                  or there is an object with such mashineId and epId
          or
            body:
		{
			"a": {
					"machineId": {"type":"string"},
					"epId": {"type":"string"},
					"timestamp": {"type":"number"},
					"measurementValue": {"type":"number"}
				  },
			"b": {
					"machineId": {"type":"string"},
					"epId": {"type":"string"},
					"timestamp": {"type":"number"},
					"measurementValue": {"type":"number"}
				 }
		},

		if there is an object in the database with this mashineId, but with a different epId
		and the time difference between them is less than 2 hours

			or
            
            body:
            {
				"a": {
					"machineId": {"type":"string"},
					"epId": {"type":"string"},
					"timestamp": {"type":"number"},
					"measurementValue": {"type":"number"}
				}
			},

		if there is an object in the database with this mashineId, but with a different epId
		and the time difference between them is more than 2 hours

		status: 200