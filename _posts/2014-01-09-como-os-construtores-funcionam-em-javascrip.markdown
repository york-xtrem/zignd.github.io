---
layout: post
title:  "Como os construtores funcionam em JavaScript"
date:   2014-01-09 17:48:00
categories: jekyll update
permalink: construtor-javascript
---

Um função construtora não precisa criar um objeto, pois quando ela é chamada através de uma chamada de contrutor (ex. new Range()) um objeto é automaticamente criado e esse construtor é executado como método desse objeto ou seja você tem acesso ao novo objeto através do this. Quando o prototype do novo objeto será o prototype do construtor.
 
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
