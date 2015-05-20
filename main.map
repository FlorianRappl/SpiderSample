$traceurRuntime.ModuleStore.getAnonymousModule(function() {
  "use strict";
  function Point() {
    var x = arguments[0] !== (void 0) ? arguments[0] : 0;
    var y = arguments[1] !== (void 0) ? arguments[1] : 0;
    var $__0 = this;
    this.x = x;
    this.y = y;
    this.add = (function(that) {
      $__0.x += that.x;
      $__0.y += that.y;
      return $__0;
    });
    this.sub = (function(that) {
      $__0.x -= that.x;
      $__0.y -= that.y;
      return $__0;
    });
    this.dist = (function(that) {
      return Math.sqrt(Math.pow($__0.x - that.x, 2) + Math.pow($__0.y - that.y, 2));
    });
  }
  function GameObject(game) {
    var location = arguments[1] !== (void 0) ? arguments[1] : new Point();
    var velocity = arguments[2] !== (void 0) ? arguments[2] : new Point();
    var acceleration = arguments[3] !== (void 0) ? arguments[3] : new Point();
    var $__0 = this;
    this.game = game;
    this.location = location;
    this.velocity = velocity;
    this.acceleration = acceleration;
    this.move = (function() {
      $__0.location.add($__0.velocity);
      $__0.velocity.add($__0.acceleration);
    });
    this.draw = (function() {});
    this.collides = (function(other) {
      if (!!(typeof $__0.radius !== "undefined" && $__0.radius !== null) && !!(typeof other.radius !== "undefined" && other.radius !== null)) {
        return $__0.radius + other.radius > $__0.location.dist(other.location);
      }
      return false;
    });
  }
  function Asteroid(game, location, velocity, radius) {
    var $__0 = this;
    GameObject.call(this, game, location, velocity);
    this.radius = radius;
    this.draw = (function() {
      var ctx = $__0.game.ctx;
      ctx.save();
      ctx.translate($__0.location.x, $__0.location.y);
      ctx.beginPath();
      ctx.arc(0, 0, $__0.radius, 0, 2 * Math.PI);
      ctx.strokeStyle = "red";
      ctx.stroke();
      ctx.restore();
    });
  }
  Asteroid.prototype = Object.create(GameObject);
  function AsteroidGenerator(game) {
    GameObject.call(this, game);
    this.move = (function() {
      if (Math.random() > 0.95) {
        var $__3 = function() {
          var forIn0 = [];
          for (var $__1 = [1, 2, 3][$traceurRuntime.toProperty(Symbol.iterator)](),
              $__2; !($__2 = $__1.next()).done; ) {
            var i = $__2.value;
            {
              forIn0.push(Math.random());
            }
          }
          return forIn0;
        }(),
            a = $__3[0],
            b = $__3[1],
            c = $__3[2];
        var radius = a * 40 + 10;
        var angle = b * 2 * Math.PI;
        var vel = c * 4 + 1;
        var dist = Math.sqrt(Math.pow(game.ctx.canvas.width, 2) + Math.pow(game.ctx.canvas.height, 2)) + radius;
        var location = new Point(Math.cos(angle) * dist, Math.sin(angle) * dist);
        var velocity = new Point(-Math.cos(angle) * vel, -Math.sin(angle) * vel);
        game.items.push(new Asteroid(game, location, velocity, radius));
      }
    });
  }
  AsteroidGenerator.prototype = Object.create(GameObject);
  function Ship(game) {
    var $__0 = this;
    GameObject.call(this, game);
    this.radius = 10;
    this.draw = (function() {
      var ctx = $__0.game.ctx;
      var alpha = Math.atan2(-$__0.velocity.x, $__0.velocity.y);
      ctx.save();
      ctx.translate($__0.location.x, $__0.location.y);
      ctx.rotate(alpha);
      ctx.beginPath();
      ctx.moveTo(0, 10);
      ctx.lineTo(10, -10);
      ctx.lineTo(-10, -10);
      ctx.closePath();
      ctx.strokeStyle = "green";
      ctx.stroke();
      ctx.restore();
    });
  }
  Ship.prototype = Object.create(GameObject);
  function ShipController(game, ship) {
    var $__0 = this;
    GameObject.call(this, game);
    this.ship = ship;
    this.up = false;
    this.down = false;
    this.left = false;
    this.right = false;
    this.change = (function(keyCode, state) {
      if (keyCode === 37) {
        $__0.left = state;
      } else if (keyCode === 38) {
        $__0.up = state;
      } else if (keyCode === 39) {
        $__0.right = state;
      } else if (keyCode === 40) {
        $__0.down = state;
      }
    });
    var da = Math.PI * 0.03;
    var ds = 0.1;
    var angle = 0;
    var total = 0;
    this.move = (function() {
      angle += $__0.right * da - $__0.left * da;
      total += $__0.up * ds - $__0.down * ds;
      if (total < 0) {
        total = 0;
      } else {
        if (total > 10) {
          total = 10;
        }
      }
      $__0.ship.velocity = new Point(-Math.cos(angle) * total, -Math.sin(angle) * total);
    });
  }
  ShipController.prototype = Object.create(GameObject);
  function Background(game) {
    var $__0 = this;
    GameObject.call(this, game);
    this.draw = (function() {
      if (typeof $__0.image !== "undefined" && $__0.image !== null) {
        var ctx = $__0.game.ctx;
        var img = $__0.image;
        var w = ctx.canvas.width;
        var h = ctx.canvas.height;
        ctx.drawImage(img, 0, 0, img.naturalWidth, img.naturalHeight, -0.5 * w, -0.5 * h, w, h);
      }
    });
  }
  Background.prototype = Object.create(GameObject);
  function Game(ctx) {
    var $__0 = this;
    this.ctx = ctx;
    this.items = [];
    this.start = (function() {
      var drawing;
      $__0.start = function() {};
      window.setInterval((function() {
        for (var $__1 = $__0.items[$traceurRuntime.toProperty(Symbol.iterator)](),
            $__2; !($__2 = $__1.next()).done; ) {
          var item = $__2.value;
          {
            item.move();
          }
        }
        for (var i = $__0.items.length - 1; i--; ) {
          for (var j = i + 1; j < $__0.items.length; j++) {
            if ($__0.items[i].collides($__0.items[j])) {
              console.log("collission");
              $__0.items.splice(j, 1);
              $__0.items.splice(i, 1);
              break;
            }
          }
        }
      }), 25);
      drawing = (function() {
        ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
        ctx.fillRect(0, 0, ctx.canvas.width, ctx.canvas.height, "black");
        ctx.save();
        ctx.translate(ctx.canvas.width * 0.5, ctx.canvas.height * 0.5);
        for (var $__1 = $__0.items[$traceurRuntime.toProperty(Symbol.iterator)](),
            $__2; !($__2 = $__1.next()).done; ) {
          var item = $__2.value;
          {
            item.draw();
          }
        }
        ctx.restore();
        window.requestAnimationFrame(drawing);
      });
      drawing();
    });
  }
  function loadImage(url) {
    return new Promise(function(fulfill, reject) {
      var img = document.createElement("img");
      img.src = url;
      img.onload = function() {
        fulfill(img);
      };
      img.onerror = function() {
        reject(img);
      };
    });
  }
  document.addEventListener("DOMContentLoaded", function() {
    var canvas,
        body,
        context,
        game,
        ship,
        background,
        controller,
        asteroids,
        resized;
    return $traceurRuntime.asyncWrap(function($ctx) {
      while (true)
        switch ($ctx.state) {
          case 0:
            canvas = document.querySelector("canvas");
            body = document.body;
            context = canvas.getContext("2d");
            game = new Game(context);
            ship = new Ship(game);
            background = new Background(game);
            controller = new ShipController(game, ship);
            asteroids = new AsteroidGenerator(game);
            resized = function() {
              canvas.width = body.clientWidth;
              canvas.height = body.clientHeight;
              canvas.style.marginLeft = -0.5 * canvas.width + "px";
              canvas.style.marginTop = -0.5 * canvas.height + "px";
            };
            $ctx.state = 5;
            break;
          case 5:
            Promise.resolve(loadImage("http://i.ytimg.com/vi/qbzFSfWwp-w/maxresdefault.jpg")).then($ctx.createCallback(3), $ctx.errback);
            return;
          case 3:
            background.image = $ctx.value;
            $ctx.state = 2;
            break;
          case 2:
            game.items.push(background);
            game.items.push(ship);
            game.items.push(controller);
            game.items.push(asteroids);
            body.onresize = resized;
            window.addEventListener("keydown", function(ev) {
              controller.change(ev.keyCode, true);
            }, false);
            window.addEventListener("keyup", function(ev) {
              controller.change(ev.keyCode, false);
            }, false);
            game.start();
            resized();
            $ctx.state = -2;
            break;
          default:
            return $ctx.end();
        }
    }, this);
  });
  return {};
});

//# sourceMappingURL=main.map
