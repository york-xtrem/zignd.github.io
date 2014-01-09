---
layout: post
title:  "Classes em JavaScript [Anotações pessoais]"
date:   2014-01-09 20:35:00
categories: jekyll update
permalink: construtor-javascript
---

Um função construtora não precisa criar um objeto, pois quando ela é chamada através de uma chamada de contrutor (ex. `new Range()`) um objeto é automaticamente criado e esse construtor é executado como método desse objeto ou seja você tem acesso ao novo objeto através da palavra chave `this`. Quando o prototype do novo objeto será o prototype do construtor.
 
{% highlight javascript %}
function Range(from, to) {
    this.from = from;
    this.to = to;
}
 
// Será utilizado como prototype de um objeto Range
Range.prototype = {
    includes: function (x) {
        return this.from <= x && x <= this.to;
    },
    foreach: function (f) {
        for (var x = Math.ceil(this.from) ; x <= this.to; x++)
            f(x);
    },
    toString: function () {
        return "(" + this.from + "..." + this.to + ")";
    }
};
 
var r = new Range(1, 3);
r.includes(2);
r.foreach(console.log.bind(console));
console.log(r);
{% endhighlight %}


Criando propriedades

Exemplo:
{% highlight javascript %}
function Complex(real, imaginary) {
    if (isNaN(real) || isNaN(imaginary)) {
        throw new TypeError();
    }

    // Instance fields, only available through an instance of the class

    this.r = real;
    this.i = imaginary;
}

// Instance methods, only available through an instance of the class

Complex.prototype.add = function (that) {
    return new Complex(this.r + that.r, this.i + that.i);
};

Complex.prototype.mul = function (that) {
    return new Complex(this.r * that.r - this.i * that.i,
        this.r * that.i + this.i * that.r);
};

Complex.prototype.mag = function () {
    return Math.sqrt(this.r * this.r + this.i * this.i);
};

Complex.prototype.neg = function () {
    return new Complex(-this.r, -this.i);
};

Complex.prototype.toString = function () {
    return "{" + this.r + "," + this.i + "}";
};

Complex.prototype.equals = function (that) {
    return that != null &&
        that.constructor === Complex &&
        this.r === that.r && this.i === that.i;
};

// Class fields, can be accessed through the class itself there is no need for an object of it

Complex.ZERO = new Complex(0, 0);
Complex.ONE = new Complex(1, 0);
Complex.I = new Complex(0, 1);

// Class fields, can be accessed through the class itself there is no need for an object of it

Complex.parse = function (s) {
    try {
        var m = Complex._format.exec(s);
        return new Complex(parseFloat(m[1]), parseFloat(m[2]));
    } catch (x) {
        throw new TypeError("Can't parse '" + s + "' as a complex number.");
    }
};

Complex._format = /^\{([^,]+),([^}]+)\}$/;

{% endhighlight %}