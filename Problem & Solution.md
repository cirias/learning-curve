### Trouble
**Q:** 使用async.compose时，发生can't setHeaders after res.send的错误
同时会发生compose中某个函数之后的的函数都被先执行的情况。
而且测试的是的时候看起来是正确的。

**A:** compose中的某个函数出现了多次执行callback的情况。
在本例中，这个函数出现了问题：

```javascript
function mergeForiegnPort(data, callback) {
  // 在async.each中删除了array的元素，这会导致多次回调
  async.each(data.shipment_routes, function (route, cb) {
    REST.foreign_ports.search(function (err, result) {
      if (err) return cb(err);

      if (!result || result.numRows == 0 || result.result.length == 0) {
        data.shipment_routes.splice(data.shipment_routes.indexOf(route), 1);
      } else {
        route.foreign_code = result.result[0].code;
      }

      cb(null, data);
    }, {
      where: JSON.stringify(["ports_by_country='" + route.ports_by_country + "'"])
    });
  }, function (err) {
    callback(err, data);
  });
}
```

```javascript
function mergeForiegnPort(data, callback) {
  // 应替换成filter
  async.filter(data.shipment_routes, function (route, cb) {
    REST.foreign_ports.search(function (err, result) {
      if (err || !result || result.numRows == 0 || result.result.length == 0) {
        cb(false);
      } else {
        route.foreign_code = result.result[0].code;
        cb(true);
      }
    }, {
      where: JSON.stringify(["ports_by_country='" + route.ports_by_country + "'"])
    });
  }, function (result) {
    data.shipment_routes = result;
    callback(null, data);
  });
}
```

------

**Q:** 线上搜索海关数据显示的公司列表中的shipments数量有时候会是0，但是测试的时侯不会发生。

**A:** 线上有多个请求在执行，而测试每次只有一个请求。当每个请求共享某些变量的时候会导致这样的问题。
在本例中，下面代码出现了问题：

```javascript
// 这两个变量被定义为模块中的全局变量，于是某次请求可能会修改另一次请求的这些变量，原本应该是不变的
var companyIdField;
var partnerIdField;
// 把他们封如请求的独立作用域中就好了
```

-------

**Q:** 使用express时，controller中的this指向了global

**A:** express的路由在绑定controller方法时，只是单纯绑定方法，并未将this一起传递，所以应该这样用

```javascript
// replace this
app.get('resource/:id', rest.retrieve);
// to
app.get('resource/:id', rest.retrieve.bind(rest));
// Note: not every method not pass this. ex.,async
```
