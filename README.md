# aws-dynamodb-local-lab
docker pull deangiberson/aws-dynamodb-local
docker run -it -p 8000:8000 deangiberson/aws-dynamodb-local 
goto http://<container_ip>:8000/shell



# create table in dynamodb
var params = {
TableName: 'student',
KeySchema: [ 
    { 
        AttributeName: 'sid',
        KeyType: 'HASH',
    },
],
AttributeDefinitions: [ 
    {
        AttributeName: 'sid',
        AttributeType: 'N', 
    },


],
ProvisionedThroughput: { 
    ReadCapacityUnits: 10, 
    WriteCapacityUnits: 10, 
},
};

dynamodb.createTable(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response

});


# write into table
var params = {
TableName: 'student',
Item: { // a map of attribute name to AttributeValue

    sid: 123,
    firstname : { 'S': 'abc' },
    lastname : { 'S': 'xyz' },
    address : {'S': 'pqr' },
    ReturnValues: 'NONE', // optional (NONE | ALL_OLD)
    ReturnConsumedCapacity: 'NONE', // optional (NONE | TOTAL | INDEXES)
    ReturnItemCollectionMetrics: 'NONE', // optional (NONE | SIZE)
};
docClient.put(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});



# get item from table
var params = {
    TableName: 'student',
    Key: { 
        sid: 1234, //(string | number | boolean | null | Binary)
        // more attributes...

    },
     AttributesToGet: [ // optional (list of specific attribute names to return)
        'firstname',
        // ... more attribute names ...
    ],
    ConsistentRead: false, // optional (true | false)
    ReturnConsumedCapacity: 'NONE', // optional (NONE | TOTAL | INDEXES)
};


docClient.get(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
