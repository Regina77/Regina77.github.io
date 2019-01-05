---
layout:     post
title:      How to Create A Table View In Swift
tags: 		Swift TableView
subtitle:  	Introduction to table view in Swift
category:  swift
---

## Some Jibber-jabber
Since I kinda have a special attachment to table views, I decided to write about it for my first post. Not only it is the first thing I learned about in Swift, but also the reason I got my second co-op job. According to my beloved boss, I jumped on top of the list when I was the only one who could clearly explain how to create a table view in the interview. It's pretty funny actually. Everyone was trying to get done as many questions as possible on LeetCode, and yet sometimes the interviewer just focus on some specific development skills. Also, a table view is what Shopify ask you to build to get an interview, along with some API handling and JSON parsing stuff. So, let's get started on learning about table views in Swift. You'll definitely need it when you apply for an iOS development job.

## What are the basic things you need to know about table views?
A functioning table view requires **three** table view data source methods.
{% highlight swift %}
    1. func numberOfSections(in tableView: UITableView) -> Int
    2. func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int
    3. func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell
{% endhighlight %}
The first function returns the number of sections, the second function returns the number of rows in each section and the third function returns the cells.<br>The first two functions are pretty straightforward. The third one can be a little complicated for beginners. Most likely, you will have something like this in your configuring cell function:
{% highlight swift %}
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cellIdentifier = "yourCellIdentifier"
    let cell = tableView.dequeueReusableCell(withIdentifier: cellIdentifier, for: indexPath)
    guard let cell = tableView.dequeueReusableCell(withIdentifier: cellIdentifier, for: indexPath) as? YourCellClass  else {
    fatalError("The dequeued cell is not an instance of YourCellClass.")
}
{% endhighlight %}<br>
Now that you know these three methods, you are completely capable of creating a basic table view. You just put this three functions in your table view controller class -- Boom! A table is built. 
## Take a further step
Although you can follow the above methods to build a perfect functioning table view, it is not the best way to do it. The view controller can be bloated if you want to add more functionalities to the table like adding rows, deleting rows, editing header or footer height in sections, etc. Here is when the Extensions comes in. You should take advantage of Swift’s protocol-oriented nature to keep you code elegant and clean by using Extensions.
### What is the Extensions & why do we want to use it?
>Extensions add new functionality to an existing **class**, **structure**, **enumeration**, or **protocol** type. This includes the ability to extend types for which you do not have access to the original source code.

The good thing about extensions is that it allows you to group things together so that your code is more readable and maintainable, which is very important for big company's project. Now, let's have a look at how to build a table using extensions.
### Create a table view using extensions
{% highlight swift %}
extension TableViewController: UITableViewDataSource {
    //Put the three required data source function here
    func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        return 0
    }
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return dataSource.numberOfItemsInSection(section)
    }
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCellWithIdentifier(String.fromClass(ImageWithTextTableViewCell), forIndexPath: indexPath) as! ImageWithTextTableViewCell
        let viewModel = dataSource.viewModelForCell(atIndexPath: indexPath)
        cell.configure(withDataSource: viewModel, delegate: viewModel.delegate)
        return cell
    }
}

extension TodoListViewController: UITableViewDelegate {
    // some adding function
    // some deleting function
    // some configuring header/footer height function
    // ...
}
{% endhighlight %}
## Conclusion
There are always more than one way to achieve what we want in programming and each one is better than the other. So, don't stop thinking when you find one solution to your problem. Keep going and you will be surprised.