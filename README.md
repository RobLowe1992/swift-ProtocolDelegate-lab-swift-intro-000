# Shopping Cart Lab 


![Drawing](http://i.imgur.com/KcAYJFV.jpg?1)  

> Role models are only of limited use. For no-one is as important, potentially powerful and as key in your life and world as you. -[Rasheed Ogunlaru](https://en.wikipedia.org/wiki/Rasheed_Ogunlaru)
 
---
## Overview

In this lab, you'll create an app that works like a shopping cart. 

![](http://i.imgur.com/2NMfm0Y.png?1)

# Instructions

With your knowledge of delegation, we will use this programming technique to allow two ViewControllers to be able to communicate to each other. Similar to the Mom & Baby relationship we created in the prior lesson.

The finished product.

![](http://i.imgur.com/U1ye2GD.png?1)

Some of this has already been built for you. 

Open the `ShoppingCart.xcworkspace` file.

Look in the `ShoppingListViewController.swift` & `Main.storyboard` file to see what it is that has been built for you. If you were to run the app, it would look like this,

![](http://i.imgur.com/ZAIaHU1.png?1)

If you hit the + button in the top left, you will be met with this screen.

![](http://i.imgur.com/XpRs4FY.png?1)

# Your Tasks

**(1)** - Open the `Main.storyboard` file and setup the Emoji Selection View Controller Scene to look like this:

![](http://i.imgur.com/KLFKx3f.png?1)

This is made up of
* One UILabel
* Two UITextFields
* One UIButton

**(2)** - Create outlets from the two UITextFields and an action for the UIButton to the 
`EmojiSelectionViewController.swift` file.

**(3)** - Head back to the `ShoppingViewController.swift` file. Near the top of this file, right below the `import UIKit` line of code, we will be creating a protocol.

Create a protocol called `EmojiCreationDelegate`. In your implementation of this protocol should be one requirement. A function called `emojiGroupCreated` that takes in one argument called `emojiGroup` of type (`String`, `String`).

If someone was to call on this function, they would do so like this:

```swift
emojiGroupCreated(("😋", "🤕"))
```

The two parenthesis seem weird when calling that function (we'll go into that more later), but that's because the type of the argument of this function is a tuple and you create a tuple using parenthesis like that.

So now that we created this protocol, lets have someone adopt and conform to it.

**(4)** In the `ShoppingViewController.swift` file, scroll down to the bottom and create an extension on the `ShoppingViewController` where within the extension you're adopting the `EmojiCreationDelegate`. Very similar to how it's being done with the Data Source and Delegate protocols above it for the Table View.

**(5)** Conform to this protocol within your newly made extension by implementing the `emojiGroupCreated(_:)` method that's part of this protocol. 

Before you start writing code, take a look near the top of this file. This Table View will display the emoji's from our array called `emojis`.

```swift
var emojis: [(String, String)] = []
```

`emojis` is an instance property of type [(`String`, `String`)] where we then assign it a value of an empty array. What's stored in this array are tuples.

We know that our `emojiGroupCreated(_:)` method takes in a tuple as its argument. So in your implementation of this method, append to this `emojis` variable the tuple passed into this function.

Right below that line of code, you need to do one more thing. We need to tell our `tableView` property to do something.. because the data it's relying on has changed! We need to tell it to reload its data. 

So type this line of code in below where you append the tuple passed in to the `emojis` property.

```swift
tableView.reloadData()
```

**(6)** - Going back to the `EmojiSelectionViewController.swift` file, we need to add a new property to this class. It should be a variable called `emojiDelegate` of type `EmojiCreationDelegate?`. Don't forget that question mark, it will be an optional `EmojiCreationDelegate`. 

**(7)** - In the `EmojiSelectionViewController.swift` file, locate the action method you created in an earlier step. When the save button is tapped, we want to call on a certain function through our `emojiDelegate` property. Can you guess which one? Well.. considering the type of `emojiDelegate` is  `EmojiCreationDelegate` we only have the ability to call on one method anyway through this property. In calling on this function, we want to pass in a tuple where the first part of the tuple represents the text typed in from the left text field, the right part of the tuple represents the text typed in from the right text field.

Considering this is a lot to do in one line of code, consider breaking this down into chunks.

First grab the text from the left text field and store that in a constant. Grab the text from the right text field and store that in a constant. Now create a new constant which is a tuple made up of those two `String`'s you just grabbed from the text fields. _Then_ call on the `emojiGroupCreated(_:)` on your `emojiDelegate` instance property passing in this new tuple you created.

Below that delegate method call (still in the implementation of your button tapped method), type this line of code as we will want to dismiss our View Controller.

```swift
dismissViewControllerAnimated(true, completion: nil)
```

If we were to now run our code, would this all work? Not yet, do you know why? If you think back to the Mother / Baby relationship in the prior reading, we established a connection between the two. Here, we've set it all up.. but we haven't yet established the connection.

**(8)** - In the `ShoppingListViewController.swift` file, we need to implement the `prepareForSegue(_:sender:)` method inherit to all `UIViewController`'s.

A reminder on how segue's work. We created this segue in the `Main.storyboard` file for you.

![](http://i.imgur.com/Q8eNidn.png?1)

The segue was setup when someone goes to tap the + button. When that button is tapped, we travel down the segue (if you want to think of the segue as a road) to its destination which is the second view controller.

So when someone taps that + button, before we jump to our destination the `prepareForSegue(_:sender:)` method is called on the View Controller from which we're leaving from (like leaving home to go to a vacation spot). 

```swift
override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        
}
```

One of the arguments to this function is called `segue` which is of type `UIStoryboardSegue`. This is what we want here. Through this `segue` object, we can find out where we we're going and communicate with that destination (the destination here is the `EmojiSelectionViewController`.

So lets get a hold of our destination view controller, like so:

```swift
let destVC = segue.destinationViewController as! EmojiSelectionViewController
```

Now that we have a hold of where we're going, `self` needs to become the `emojiDelegate` of the second view controller. Think of the left View Controller as the parent, and the right one as its child.

We setup this connection like so:

```swift
destVC.emojiDelegate = self
```

Now everything is connected and when the SAVE button is tapped, it should communicate through its delegate (which is the first View Controller) that a new emojiGroup has been added, it will then call on that method you implemented earlier where we add that group to the array and reload the table view.

Congrats!

Feel free to `print()` statements throughout the various functions here to confirm that everything is getting called correctly and in what order. This is an important and powerful concept to understand.

<a href='https://learn.co/lessons/ProtocolDelegate' data-visibility='hidden'>View this lesson on Learn.co</a>
