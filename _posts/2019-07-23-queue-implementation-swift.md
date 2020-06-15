---
layout: post
title:  Queue implementation in Swift
date:   2019-07-24 15:01:35 +0300
image:  './assets/img/queue/queue1.jpg'
tags:   [Queue, Data Structure, Swift]
---

Whenever we go for shopping or for some dinning experience, we usually have to stand in queue and wait for out turn. Amid the COVID-19 Pandemics, we can now see the queue being normal around us.

As in every queue, the one who goes in first, comes out first, it is similar with Queue Data Structure. We call it First in First out (FIFO), and we are going to implement it using array in Swift. 

![](/assets/img/queue/queue2.jpg)

We are going to create a basic class in Swift Playground. It is going to be a generic queue, it will facilitate to create queue of any data types. We are writing an array of the generic type “T", which will hold our data. We have to initialize the queue as well.

Next thing, is declaring a variable to check, if the array is empty. Since, we cannot dequeue an empty array. A queue also needs a peek method which returns the item at the beginning of the queue without removing it.

![](/assets/img/queue/queue3.jpg)

## Enqueue
Now, we need to add values to our queue, it needs an enqueue method, which will add the object to the end of the Queue. This method will mutate the array, we have to explicitly specify it while declaring the enqueue method. 

## Dequeue
Similarly, we need to dequeue the queue, we will add a dequeue method that returns the first item in the queue. The return type is nullable to handle the queue being empty.

![](/assets/img/queue/queue4.jpg)

Voila! our queue is ready, we are going to test it with Integer data.

Write an extension to print out the readable output string, you can make Queue adopt the **CustomStringConvertible** Protocol.

![](/assets/img/queue/queue5.jpg)

## Implementing with Example
Outside the implementation of Queue, write the following to enqueue data in our queue, declare Integer type to append integer values in the array. 
You can play with it by changing it to String and creating a queue of string type as well.

![](/assets/img/queue/queue6.jpg)

Here, we have enqueued and dequeued the queue.

When you enqueue, the result is : [11, 22, 33, 44]
When you dequeue, since we are popping out the first element of the queue,  the result is : [22, 33, 44]

Complete Code:

https://gist.github.com/hempoudyal/cec3cdbfcdf2bb4a1d309a95210d063b#file-queue-swift

