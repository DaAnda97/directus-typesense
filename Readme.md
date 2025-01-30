## Directus with Typesense

1. Run stack
    ``` bash
    docker-compose up -d
    ```

2. Access Directus at http://localhost:8055

3. Create a new entry in "Search Engine Configs" Collection

Engine Types: Typesense

Typesense URLs: http://typesense:8108

Typesense Schema:
- Status: Published
- Schema Name: users
- Collection: directus_users
- Mode: Trigger Event
- Query:
``` json
{
    "fields": [
        "*"
    ]
}
```
- Function Parse:
``` javascript
module.exports = function(data) {
    const escapeHtml = (text) => text ? text.replace(/(<([^>]+)>)/gi, '') : text;
	console.log(data)
    return data.map(item => ({
        id: item.id.toString(),
        email: item.email,
        first_name: item.first_name,
        last_name: item.last_name,
        date_created: item.date_created ? new Date(item.date_created).getTime() : null,
        date_updated: item.date_updated ? new Date(item.date_updated).getTime() : null
    }));
};
```
- Schema:
``` json
{
    "fields": [
        {
            "name": "id",
            "type": "string"
        },
        {
            "name": "email",
            "type": "string"
        },
        {
            "name": "first_name",
            "type": "string"
        },
        {
            "name": "last_name",
            "type": "string"
        },
        {
            "name": "date_created",
            "type": "int64",
            "optional": true
        },
        {
            "name": "date_updated",
            "type": "int64",
            "optional": true
        }
    ],
    "token_separators": [
        "+",
        "-",
        "@",
        "."
    ]
}
```