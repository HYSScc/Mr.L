教程：[api-blueprint\/Tutorial.md at master · apiaryio\/api-blueprint · GitHub](https://github.com/apiaryio/api-blueprint/blob/master/Tutorial.md)

文档：[api-blueprint\/API Blueprint Specification.md at master · apiaryio\/api-blueprint · GitHub](https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md)





> {
> 
> "required": true,
> 
> "$schema": "[http:\/\/json-schema.org\/draft-03\/schema](http://json-schema.org/draft-03/schema)",
> 
> "type": "object",
> 
> "properties": {
> 
> "code": {
> 
> "type": "number",
> 
> "required": true
> 
> },
> 
> "msg": {
> 
> "required": true,
> 
> "type": "string"
> 
> },
> 
> "data": {
> 
> "required": false,
> 
> "type": "array",
> 
> "items": \[
> 
> {
> 
> "type": "object",
> 
> "properties": {
> 
> "score": {
> 
> "required": false,
> 
> "type": "string"
> 
> },
> 
> "city": {
> 
> "required": false,
> 
> "type": "string"
> 
> },
> 
> "coordinate": {
> 
> "type": "array",
> 
> "required": false,
> 
> "items": {
> 
> "type": "number"
> 
> }
> 
> },
> 
> "\_id": {
> 
> "required": false,
> 
> "type": "string"
> 
> },
> 
> "dev\_img": {
> 
> "required": false,
> 
> "type": "string"
> 
> },
> 
> "name": {
> 
> "required": false,
> 
> "type": "string"
> 
> },
> 
> "description": {
> 
> "required": false,
> 
> "type": "string"
> 
> }
> 
> }
> 
> }
> 
> \]
> 
> }
> 
> }
> 
> }

