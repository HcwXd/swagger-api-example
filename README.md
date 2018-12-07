# swagger-api-example

> [Live Demo](https://hcwxd.github.io/swagger-api-example/index.html)
>
> The render template this demo used is made by [redoc](https://github.com/Rebilly/ReDoc/blob/master/cli/README.md). 
>
> You can alse upload the `swagger.json` to [swagger editor](https://editor.swagger.io/#) to see the official render template. (Please ignore the errors when uploading to swagger editor)
>
> For those who using vs code, you can easily preview the demo when editing `swagger.json` by installing the Swagger Viewer plugin. 



## Installation

```
git clone https://github.com/HcwXd/swagger-api-example.git
cd ./swagger-api-example
```



## Learn swagger through the example

1. Open `index.html` to see what's the finished document look like
2. Open `swagger.json` which contains all the basics syntax needed for writing a simple swagger document



## Learn swagger step by step 

### Swagger Structure Overview

A swagger document can be divided into three parts:

1. Metadata
   - Metadata of api document, including api version, test host, api category, etc.
2. Api endpoints
   - Define each api endpoints method, request, response.
3. Reusable component
   - Define component that can be reused for convenience when making document

### Metadata

```json
"swagger": "2.0",
"info": {
    "version": "1.0.0",
    "title": "Api Document Example",
    "description": "這是一個學習如何使用 Swagger 產生 API Document 的範例。"
},
"schemes": ["http"],
"host": "myTestServer",
"basePath": "'v1",
"tags": [
    {
        "name": "learnRequest",
        "description": "在這個分類學習如何描述 Request"
    },
    {
        "name": "learnResponse",
        "description": "在這個分類學習如何描述 Response"
    }
],
```

- `swagger` : The swagger version
- `info`
  - `version`: The api version
  - `title`: The title of the document
  - `description` : Description of the document
    - It's noteworthy that you can actually place description everywhere in a swagger document
- Test url
  - `schemes` ,`host`, `basePath `
  - The three above determine the url that get the testing request
  - For example, the above config send testing request to `http://myTestServer/v1`
- `tags`: Define the catagory of different api

### Api endpoints

#### Basic Example

```json
"paths": {
    "/haveSomeParams/{someQuery}": {
        "get": {
            "tags": ["learnRequest"],
            "summary": "如何描述在 query 中的參數",
            "description": "如何描述在 query 中的參數",
            "produces": ["application/json"],
            "parameters": [
                {
                    "in": "query",
                    "name": "someQuery",
                    "description": "參數在 query 中的例子",
                    "required": true,
                    "type": "string",
                    "schema": {
                        "type": "string",
                        "example": "yolo!"
                    }
                }
            ]
        }
    },
```

- `paths`
  - Each api endpoint is defined under this level
  - The method is defined after the api endpoint
- `tags`: The catagory this api endpoint belong to
- `produces`: The format of the output
- `parameters`
  - Each parameter have several properties
  - `in` : Can be header, body, query, etc
  - `name`: Name of the parameter
  - `required`: Whether the parameter is required or optional
  - `type`: The type of the parameter
  - `schema`: The schema of the parameter, you can put example request parameter here. Also, you can put description here for more clarity

#### Request with different types in parameter

```json
"/haveSomeParams": {
    "post": {
        "tags": ["learnRequest"],
        "summary": "如何描述在 body 或 header 的參數",
        "description": "如何描述在 body 或 header 的參數",
        "consumes": ["application/json"],
        "produces": ["application/json"],
        "parameters": [
            {
                "in": "body",
                "name": "body",
                "description": "參數在 body 中的例子",
                "required": true,
                "type": "object",
                "schema": {
                    "properties": {
                        "propString": {
                            "type": "string",
                            "example": "abcd",
                            "description": "這是一個字串的參數"
                        },
                        "propInt": { "type": "int", "example": 123, "description": "這是一個整數的參數" },
                        "propBoolean": {
                            "type": "boolean",
                            "example": false,
                            "description": "這是一個布林值的參數"
                        }
                    }
                }
            },
            {
                "name": "authorization",
                "in": "header",
                "required": false,
                "description": "參數在 header 中的例子，此處以 authorization 為例",
                "type": "string",
                "schema": {
                    "type": "string",
                    "example": "acde1jflk34fd21ekfkf",
                    "description": "這是一串秘密的授權碼"
                }
            }
        ]
    }
},
```

- As you can see, you can define object type of parameter by declaring its properties in schema
- And also, you can define parameter in header.

#### Define response

```json
"/haveSomeResponse": {
    "get": {
        "tags": ["learnResponse"],
        "summary": "如何描述不同的 Response",
        "description": "如何描述不同的 Response",
        "consumes": ["application/json"],
        "produces": ["application/json"],
        "responses": {
            "200": {
                "description": "這是一個成功 Response 的例子",
                "type": "object",
                "schema": {
                    "properties": {
                        "msg": {
                            "type": "string",
                            "example": "success",
                            "description": "成功 Response 在這裡會回傳 success"
                        },
                        "logCode": {
                            "type": "int",
                            "example": 2357,
                            "description": "這是一個成功 Response 後的追蹤 log code"
                        }
                    }
                }
            },
            "400": {
                "description": "缺少參數或參數格式不正確",
                "type": "object",
                "schema": {
                    "properties": {
                        "error": {
                            "type": "string",
                            "example": "params are invalid",
                            "description": "失敗 Response 的 error message"
                        }
                    }
                }
            }
        }
    }
},
```

- `responses`
  - Define response here followed by the status code

### Reusable component

You can define reusable component for convenience when making document

```json
"definitions": {
    "successResponseSchema": {
        "properties": {
            "msg": {
                "type": "string",
                "example": "success",
                "description": "成功 Response 在這裡會回傳 success"
            },
            "logCode": {
                "type": "int",
                "example": 2357,
                "description": "這是一個成功 Response 後的追蹤 log code"
            }
        }
    },
    "headerParamExample": {
        "name": "authorization",
        "in": "header",
        "required": false,
        "description": "Example params in header",
        "type": "string",
        "schema": {
            "type": "string",
            "example": "acde1jflk34fd21ekfkf"
        }
    }
}
```

- `definitions`
  - You define your component under this level
    - As you can see, you can define any kind of component in every level
    - While `successResponseSchema` define component at schema level, `headerParamExample` define at parameter level
  - You use definitions by `"$ref": "#/definitions/yourComponentName"` in the document

#### Before

```json
"/haveSomeResponse": {
    "get": {
        "tags": ["learnResponse"],
        "summary": "如何描述不同的 Response",
        "description": "如何描述不同的 Response",
        "consumes": ["application/json"],
        "produces": ["application/json"],
        "responses": {
            "200": {
                "description": "這是一個成功 Response 的例子",
                "type": "object",
                "schema": {
                    "properties": {
                        "msg": {
                            "type": "string",
                            "example": "success",
                            "description": "成功 Response 在這裡會回傳 success"
                        },
                        "logCode": {
                            "type": "int",
                            "example": 2357,
                            "description": "這是一個成功 Response 後的追蹤 log code"
                        }
                    }
                }
            },
            "400": {
                "description": "缺少參數或參數格式不正確",
                "type": "object",
                "schema": {
                    "properties": {
                        "error": {
                            "type": "string",
                            "example": "params are invalid",
                            "description": "失敗 Response 的 error message"
                        }
                    }
                }
            }
        }
    }
},
```

#### After

```json
"/haveSomeParams/withDefinition": {
    "post": {
        "tags": ["learnDefinition"],
        "summary": "如何使用 definition 在 Param 層級描述 Request",
        "description": "definition 可以描述各種層級，也可以直接在參數層級定義 definition",
        "consumes": ["application/json"],
        "produces": ["application/json"],
        "parameters": [
            {
                "$ref": "#/definitions/bodyParamExample"
            },
            {
                "$ref": "#/definitions/headerParamExample"
            }
        ]
    }
}
}
```

```json
"definitions": {
    "bodyParamExample": {
        "in": "body",
        "name": "body",
        "description": "參數在 body 中的例子",
        "required": true,
        "type": "object",
        "schema": {
            "properties": {
                "propString": {
                    "type": "string",
                    "example": "abcd",
                    "description": "這是一個字串的參數"
                },
                "propInt": { "type": "int", "example": 123, "description": "這是一個整數的參數" },
                "propBoolean": {
                    "type": "boolean",
                    "example": false,
                    "description": "這是一個布林值的參數"
                },
                "propArray": {
                    "type": "array",
                    "example": [1, 2, 3],
                    "description": "這是一個陣列的參數"
                },
                "propObject": {
                    "type": "object",
                    "example": {
                        "prop1": 1234,
                        "prop2": "abcde",
                        "prop3": true
                    },
                    "description": "這是一個物件的參數"
                }
            }
        }
    },
    "headerParamExample": {
        "name": "authorization",
        "in": "header",
        "required": false,
        "description": "Example params in header",
        "type": "string",
        "schema": {
            "type": "string",
            "example": "acde1jflk34fd21ekfkf"
        }
    }
}

```

### The whole document in the example

```json
{
    "swagger": "2.0",
    "info": {
        "version": "1.0.0",
        "title": "Api Document Example",
        "description": "這是一個學習如何使用 Swagger 產生 API Document 的範例。"
    },
    "schemes": ["http", "https"],
    "host": "localhost",
    "basePath": "",
    "tags": [
        {
            "name": "learnRequest",
            "description": "在這個分類學習如何描述 Request"
        },
        {
            "name": "learnResponse",
            "description": "在這個分類學習如何描述 Response"
        }
    ],

    "paths": {
        "/haveSomeParams/{someQuery}": {
            "get": {
                "tags": ["learnRequest"],
                "summary": "如何描述在 query 中的參數",
                "description": "如何描述在 query 中的參數",
                "produces": ["application/json"],
                "parameters": [
                    {
                        "in": "query",
                        "name": "someQuery",
                        "description": "參數在 query 中的例子",
                        "required": true,
                        "type": "string",
                        "schema": {
                            "type": "string",
                            "example": "yolo!"
                        }
                    }
                ]
            }
        },
        "/haveSomeParams": {
            "post": {
                "tags": ["learnRequest"],
                "summary": "如何描述在 body 或 header 的參數",
                "description": "如何描述在 body 或 header 的參數",
                "consumes": ["application/json"],
                "produces": ["application/json"],
                "parameters": [
                    {
                        "in": "body",
                        "name": "body",
                        "description": "參數在 body 中的例子",
                        "required": true,
                        "type": "object",
                        "schema": {
                            "properties": {
                                "propString": {
                                    "type": "string",
                                    "example": "abcd",
                                    "description": "這是一個字串的參數"
                                },
                                "propInt": { "type": "int", "example": 123, "description": "這是一個整數的參數" },
                                "propBoolean": {
                                    "type": "boolean",
                                    "example": false,
                                    "description": "這是一個布林值的參數"
                                }
                            }
                        }
                    },
                    {
                        "name": "authorization",
                        "in": "header",
                        "required": false,
                        "description": "參數在 header 中的例子，此處以 authorization 為例",
                        "type": "string",
                        "schema": {
                            "type": "string",
                            "example": "acde1jflk34fd21ekfkf",
                            "description": "這是一串秘密的授權碼"
                        }
                    }
                ]
            }
        },

        "/haveSomeResponse": {
            "get": {
                "tags": ["learnResponse"],
                "summary": "如何描述不同的 Response",
                "description": "如何描述不同的 Response",
                "consumes": ["application/json"],
                "produces": ["application/json"],
                "responses": {
                    "200": {
                        "description": "這是一個成功 Response 的例子",
                        "type": "object",
                        "schema": {
                            "properties": {
                                "msg": {
                                    "type": "string",
                                    "example": "success",
                                    "description": "成功 Response 在這裡會回傳 success"
                                },
                                "logCode": {
                                    "type": "int",
                                    "example": 2357,
                                    "description": "這是一個成功 Response 後的追蹤 log code"
                                }
                            }
                        }
                    },
                    "400": {
                        "description": "缺少參數或參數格式不正確",
                        "type": "object",
                        "schema": {
                            "properties": {
                                "error": {
                                    "type": "string",
                                    "example": "params are invalid",
                                    "description": "失敗 Response 的 error message"
                                }
                            }
                        }
                    }
                }
            }
        },
        "/haveSomeResponse/withDefinition": {
            "get": {
                "tags": ["learnDefinition"],
                "summary": "如何使用 definition 在 Schema 層級描述 Response",
                "description": "definition 是可以重複使用的物件簡寫，可以用 `\"$ref\" : \"#/definitions/modelName\"` 存取\n",
                "consumes": ["application/json"],
                "produces": ["application/json"],
                "responses": {
                    "200": {
                        "description": "這是一個成功 Response 的例子",
                        "schema": { "$ref": "#/definitions/successResponseSchema" }
                    },
                    "400": {
                        "description": "缺少參數或參數格式不正確",
                        "schema": { "$ref": "#/definitions/invalidRequestResponseSchema" }
                    },
                    "401": {
                        "description": "帳號或密碼錯誤",
                        "schema": {
                            "$ref": "#/definitions/loginFailResponseSchema"
                        }
                    }
                }
            }
        },
        "/haveSomeParams/withDefinition": {
            "post": {
                "tags": ["learnDefinition"],
                "summary": "如何使用 definition 在 Param 層級描述 Request",
                "description": "definition 可以描述各種層級，也可以直接在參數層級定義 definition",
                "consumes": ["application/json"],
                "produces": ["application/json"],
                "parameters": [
                    {
                        "$ref": "#/definitions/bodyParamExample"
                    },
                    {
                        "$ref": "#/definitions/headerParamExample"
                    }
                ]
            }
        }
    },
    "definitions": {
        "successResponseSchema": {
            "properties": {
                "msg": {
                    "type": "string",
                    "example": "success",
                    "description": "成功 Response 在這裡會回傳 success"
                },
                "logCode": {
                    "type": "int",
                    "example": 2357,
                    "description": "這是一個成功 Response 後的追蹤 log code"
                }
            }
        },
        "invalidRequestResponseSchema": {
            "properties": {
                "error": {
                    "type": "string",
                    "example": "params are invalid",
                    "description": "失敗 Response 的 error message"
                }
            }
        },
        "loginFailResponseSchema": {
            "properties": {
                "error": {
                    "type": "string",
                    "example": "Unauthorized",
                    "description": "失敗 Response 的 error message"
                }
            }
        },
        "bodyParamExample": {
            "in": "body",
            "name": "body",
            "description": "參數在 body 中的例子",
            "required": true,
            "type": "object",
            "schema": {
                "properties": {
                    "propString": {
                        "type": "string",
                        "example": "abcd",
                        "description": "這是一個字串的參數"
                    },
                    "propInt": { "type": "int", "example": 123, "description": "這是一個整數的參數" },
                    "propBoolean": {
                        "type": "boolean",
                        "example": false,
                        "description": "這是一個布林值的參數"
                    },
                    "propArray": {
                        "type": "array",
                        "example": [1, 2, 3],
                        "description": "這是一個陣列的參數"
                    },
                    "propObject": {
                        "type": "object",
                        "example": {
                            "prop1": 1234,
                            "prop2": "abcde",
                            "prop3": true
                        },
                        "description": "這是一個物件的參數"
                    }
                }
            }
        },
        "headerParamExample": {
            "name": "authorization",
            "in": "header",
            "required": false,
            "description": "Example params in header",
            "type": "string",
            "schema": {
                "type": "string",
                "example": "acde1jflk34fd21ekfkf"
            }
        }
    }
}

```



## Useful Resources

- Tutorial
  - https://apihandyman.io/writing-openapi-swagger-specification-tutorial-part-2-the-basics/
- Map of open api
  - http://openapi-map.apihandyman.io/?version=2.0