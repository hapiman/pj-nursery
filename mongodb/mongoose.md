关于**$geoNear**的使用

首先，定义Model
```javascript
exports = module.exports = function(app, mongoose) {

var citySchema = new mongoose.Schema({
  name: { type: String },
  kind: { type: String },
  style: { type: String },
  loc: {
    type: {
      type: "String",
      required: true,
      enum: ['Point', 'LineString', 'Polygon'],
      default: 'Point'
    },coordinates: [Number]
  },
}, { collection: 'city_json' });
citySchema.index({ 'loc': '2dsphere' });
mongoose.model('City', citySchema,'city_json');

};
```

如何使用
```javascript

var mongoose = require('mongoose');
var City  = mongoose.model('City');
var Pet  = mongoose.model('Pet');

exports.findClosePlaces = function (req, res) {
Pet.findOne({
    id_mascota: req.params.pet
}, function (error, response) {
      if (error || !response) {
          res.status(404).send({
              status: 401,
              message: 'not found'
          });
      } else {
          var query = City.find({
            "loc": {
              "$geoNear": {
                  "type": "Point",
                  "coordinates": response.loc.coordinates,
                  "spherical": true,
                  "maxDistance": 1
              }
            }
          });
          query.exec(function(err,docs) {
            if (err) throw err;
            res.send({
                success: true,
                pet:response,
                docs: docs
            });
            console.log('success');
          });
      }
});
}
```
另一段代码
```javascript

  var mongoose = require('mongoose');
  var Schema = mongoose.Schema;

  var Place = new Schema({
      name: String,
      coordinates: { type: [Number], index: '2dsphere' }
  });

  Place.statics.findNear = function(coords, cb) {
      this.geoNear({type: 'Point', coordinates: coords}, {maxDistance: 1000, spherical: true}, cb);
  };

  module.exports = mongoose.model('Place', Place);
```
