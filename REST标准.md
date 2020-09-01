# REST标准 #

1、动作
- GET ：请求从服务器获取特定资源。举个例子：GET /classes（获取所有班级）
- POST ：在服务器上创建一个新的资源。举个例子：POST /classes（创建班级）
- PUT ：更新服务器上的资源（客户端提供更新后的整个资源）。举个例子：PUT /classes/12（更新编号为 12 的班级）
- DELETE ：从服务器删除特定的资源。举个例子：DELETE /classes/12（删除编号为 12 的班级）
- PATCH ：更新服务器上的资源（客户端提供更改的属性，可以看做作是部分更新），使用的比较少，这里就不举例子了。


命名
网址中不能有动词，只能有名词，API 中的名词也应该使用复数
如果 API 调用并不涉及资源（如计算，翻译等操作）的话，可以用动词
```
GET    /classes：列出所有班级
POST   /classes：新建一个班级
GET    /classes/classId：获取某个指定班级的信息
PUT    /classes/classId：更新某个指定班级的信息（一般倾向整体更新）
PATCH  /classes/classId：更新某个指定班级的信息（一般倾向部分更新）
DELETE /classes/classId：删除某个班级
GET    /classes/classId/teachers：列出某个指定班级的所有老师的信息
GET    /classes/classId/students：列出某个指定班级的所有学生的信息
DELETE classes/classId/teachers/ID：删除某个指定班级下的指定的老师的信息
```
状态码
2xx：成功	3xx：重定向		4xx：客户端错误	5xx：服务器错误
200 成功		301 永久重定向	400 错误请求		500 服务器错误
201 创建		304 资源未修改	401 未授权		502 网关错误
							403 禁止访问		504 网关超时
							404 未找到	
							405 请求方法不对	