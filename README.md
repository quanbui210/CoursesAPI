### API based url

```https://dev183695.service-now.com/api/now```

  

### Configuration

CORS rule

- REST API: Table API [now/table], Attachment API [now/attachment]

- Domain: localhost:5173, https://course-hub-service-dmsgf6bvt-quanbui210s-projects.vercel.app, https://course-hub-service-now.vercel.app/

- HTTP Methods: GET, POST, PUT, DELETE

  

Access Control List
Elevated to ```security_admin``` role
```
Course Subscription x_1540387_course_s_course_subscription
Type: record
Operation: delete
Admin overrides: true
Active: true
Security Attribute Conditon
- UserIsAuthenticated: true
Data Condition
- SysId === Learner.SysId
- Course === Course (Course field)
```

```
Course Subscription x_1540387_course_s_course_subscription
Type: record
Operation: create
Admin overrides: true
Active: true
Security Attribute Conditon
- UserIsAuthenticated: true
Data Condition
- Learner is not empty
- Course is not empty
```
### Tables

(Type, required, max length)

  
  

**Course**

- Title (string, true, 100)

- Description (string, false, 300)

- Course Image (Image, false, 1)

- Duration (Integer, false, 40)

  

**Course Subscription**

- Learner (Reference<"Learner">, true)

- Course (Reference<"Course">, true)

- Subscription Date ( (empty), false)

  

**Learner**

- User Account (Reference<"User">, true)

- Adminssion Date (Date/Time)

  
  
  

### Endpoints

*Using ServiceNow TableAPI for creating records for Courses, Learners and Course Subscriptions, ServiceNow Attachment API for course images*

  

*Authentication*

```

auth: {

username: "admin",

password: "****"

}

```

  

- Get all records

  

*Retrieve list of courses and subscriptions*

```

REQUEST:

GET https://dev183695.service-now.com/api/now/table/x_1540387{tableName}?sysparm_limit=10

```

  

```JSON

Example response - course

200 OK

{

"result": [

{

"sys_mod_count": "1",

"description": "Learn essential data structures and algorithms, including arrays, linked lists, trees, graphs, sorting, and searching techniques, to solve computational problems efficiently.",

"sys_updated_on": "2024-09-26 05:42:05",

"sys_tags": "",

"title": "Data Structures and Algorithms",

"type": "Hybrid",

"course_image": "228e39fe83fc92107e535855eeaad3fc",

"sys_class_name": "x_1540387_course_s_course",

"duration": "15",

"number": "COU0001003",

"sys_id": "3a1dcdee83b452107e535855eeaad319",

"sys_updated_by": "admin",

"sys_created_on": "2024-09-25 07:27:55",

"sys_created_by": "admin"

},

...others  item

]

}

```

  

- Get one record

```

REQUEST:

GET https://dev183695.service-now.com/api/now/table/{tableName}/{sys_id}

  

Headers

Content-Typeapplication/json

Acceptapplication/json

```

``` JSON

Example response - course

200 OK

{

"result": {

"sys_mod_count": "1",

"description": "Learn essential data structures and algorithms, including arrays, linked lists, trees, graphs, sorting, and searching techniques, to solve computational problems efficiently.",

"sys_updated_on": "2024-09-26 05:42:05",

"sys_tags": "",

"title": "Data Structures and Algorithms",

"type": "Hybrid",

"course_image": "228e39fe83fc92107e535855eeaad3fc",

"sys_class_name": "x_1540387_course_s_course",

"duration": "15",

"number": "COU0001003",

"sys_id": "3a1dcdee83b452107e535855eeaad319",

"sys_updated_by": "admin",

"sys_created_on": "2024-09-25 07:27:55",

"sys_created_by": "admin"

}

}

```

  

- Create a record

  

*Subscribe to a course, create a course*

  

```

POST https://dev183695.service-now.com/api/now/table/{tableName}

  

Request body:

{"learner":sys_id,"course":sys_id}

```

  

```JSON

Example response - subscription

201 CREATED

{

"result": {

"sys_id": "3c819bb283f4d2107e535855eeaad317",

"sys_updated_by": "admin",

"sys_created_on": "2024-09-26 11:44:35",

"learner": {

"link": "https://dev183695.service-now.com/api/now/table/x_1540387_course_s_learner/be82abf03710200044e0bfc8bcbe5d1c",

"value": "be82abf03710200044e0bfc8bcbe5d1c"

},

"sys_mod_count": "0",

"course": {

"link": "https://dev183695.service-now.com/api/now/table/x_1540387_course_s_course/228e39fe83fc92107e535855eeaad3fc",

"value": "228e39fe83fc92107e535855eeaad3fc"

},

"sys_updated_on": "2024-09-26 11:44:35",

"sys_tags": "",

"sys_created_by": "admin",

"subscription_date": ""

}

}

```

  

- Delete a record

  

*Unsubscribe a course*

  

```

REQUEST

  

DELETE https://dev183695.service-now.com/api/now/table/{tableName}/{sys_id}

```

  

```

Example response - subscription

  

204 No Content

""

```

  

- Get attachment

  

*Retrieve course image*

  

```

REQUEST

GET https://dev183695.service-now.com/api/now/attachment/{sys_id}

```

  

```JSON

Example response - course_image

200 OK

{

"result": {

"size_bytes": "55001",

"file_name": "course_image",

"sys_mod_count": "3",

"average_image_color": "#e7cc57",

"image_width": "1200",

"sys_updated_on": "2024-09-25 19:14:33",

"sys_tags": "",

"table_name": "ZZ_YYx_1540387_course_s_course",

"sys_id": "96eeafea833492107e535855eeaad312",

"image_height": "630",

"sys_updated_by": "admin",

"download_link": "https://dev183695.service-now.com/api/now/attachment/96eeafea833492107e535855eeaad312/file",

"content_type": "image/jpeg",

"sys_created_on": "2024-09-25 19:14:33",

"size_compressed": "46097",

"compressed": "true",

"state": "available",

"table_sys_id": "c37e232e833492107e535855eeaad3ed",

"chunk_size_bytes": "700000",

"hash": "f2b764efe9c42362793db21b9cf32dfc8e86dd27399debbd9a62bd74e2b58584",

"sys_created_by": "admin"

}

}
```
