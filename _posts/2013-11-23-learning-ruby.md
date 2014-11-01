---
layout: post
title: Learning Ruby
categories: posts
date: 2013-11-23 11:00:00
summary: In the days leading up to teaching 6th grade Math Problem Solving, I met with the teacher that I was replacing. During our lunch chat, she told me a mantra that she uses in her classroom:"learning is about being wrong before being right."
---

In the days leading up to teaching 6th grade Math Problem Solving, I met with the teacher that I was replacing. During our lunch chat, she told me a mantra that she uses in her classroom: _\"learning is about being wrong before being right.\"_ You cannot learn something new if you already know what you are doing. You must first not know what you are doing before mastering something.

This is a mantra that I repeat to myself a lot at Launch Academy because I am learning tons of new material. As a result, I am \"wrong\" a lot. However, the time spent getting introduced to new material with Johnny and Jason and then working in pairs with Alnoor this week has allowed me to be \"right\" more. Being \"right\" means I am creating more efficient code.

Using nested hashes and arrays to store and access data is an example of me being \"wrong\" before being \"right.\"

**Being \"Wrong\" (less efficient):**

Creating a cashier program, I first set up a hash with a key-value pair of the key being the Product ID and the value being an array of all the information needed for the Product (name and price).

{% highlight ruby %}
@price_list = {
  '1' => ['Light Bag', 5.0]
  '3' => ['Bold Bag', 9.75]
}
{% endhighlight %}

This got the job done, but made it incredibly confusing to access the values inside my hash later in the program. For example, to access the cost of a \"Light Bag\", I have to type @price_list[\"1\"][1] because the price of the \"Light Bag\" is the second element in the array. This works because I know that the second element in the array for the key \"1\" is the price (writing that sentence is confusing in itself for me, who wrote and executed the program). This ultimately is confusing and not efficient.

**Being \"Right\" (more efficient):**

I can set up my data using a hash of hashes to be able to access the same data in a more organized and cleaner manner. This makes data creation and retrieval more efficient.

{% highlight ruby %}
@price_list = {
  '1' => { name: 'Light Bag', price: 5.0 }
  '2' => { name: 'Medium Bag', price: 7.5 }
  '3' => { name: 'Bold Bag', price: 9.75 }
}
{% endhighlight %}

Using hashes of hashes is more efficient in this case because I can retrieve the price of product 1 by typing @price_list[\"1\"][:price]. Thus, instead of relying on knowing the position of the data I want in my array, I can reference the key :price inside the hash of the hash, which has the price of the item as the value. For this example, I no longer have to know array positional values, I just have to know the key, which should be something that makes sense.

**Although these two ways of writing and accessing data yield the same result, the second way is more efficient and a lot less confusing to keep track of. I need to continue striving towards more efficient/cleaner code so that I can continue to move from being \"wrong\" to being \"right\".**
