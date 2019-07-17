# Grail Diary Part 2 - Delegates and Protocols

A student that completes this project shows that they can:

- understand and explain what a protocol is and common scenarios for their use
- define a custom protocol, and make a class or struct conform to it
- understand and explain the purpose of UITableViewController
- use a regular UIViewController to display a UITableView
- create a custom UITableViewCell
- understand and explain the delegate pattern and why it is used

## Introduction

Grail Diary is an app that allows the user to track locations of interest (POIs) and useful facts about those locations.

## Instructions

Please use the same repository you used yesterday for Grail Diary Part 1. If you've created a PR to submit your work, you'll continue to use the same PR. New commits will automatically be added to that PR. Just submit the same link again at the end of this assignment.

### File Tasks

1. Create a new file using the Cocoa Touch Class template; call it `POIsTableViewController` and use the parent class `UITableViewController`
2. Create 2 more files in the same way; `AddPOIViewController` (subclassed from `UIViewController`) and `POIDetailViewController` (also subclassed from `UIViewController`)
3. Create a class subclassed from `UITableViewCell` called `POITableViewCell`
4. Create a Swift file called `POI` (this is your model)

### Storyboard Tasks

5. Set the custom class for each view controller scene in the storyboard to match the names of the files you just created

### Code Tasks

#### In `POI.swift`:

6. Create a `struct` called `POI` that models 3 properties: `location`, `country`, and `clues`; the data types are `String`, `String`, and an array of `String` objects; make the struct conform to the `Equatable` protocol

#### In `POIsTableViewController.swift`:

7. Create an array property to store your `POI` models
8. Create an `IBOutlet` to link the table view to your code; wire up this outlet to the table view in the storyboard
9. In an extension, make this class conform to the `UITableViewDataSource` protocol
10. Implement the following protocol methods: `tableView(_:numberOfRowsInSection:)` and `tableView(_:cellForRowAt:indexPath:)`

#### In `AddPOIViewController.swift`:

11. Declare `IBOutlet` properties for the 5 textfields in this view; wire these up to their appropriate textfield in the storyboard
12. Declare a protocol above this class in the same file called `AddPOIDelegate`; declare a single function requirement called `poiWasAdded(_ poi: POI)`
13. Declare a variable property called `delegate` of type `AddPOIDelegate` and make it optional
14. Create 2 `IBAction` methods: `cancelTapped(_:)`, and `saveTapped(_:)`; wire them up to their appropriate bar button item in the storyboard
15. Inside `cancelTapped`, simply dismiss this view as the user has indicated they want to cancel if this button is tapped
16. Inside `saveTapped`, guard unwrap the text inside the location and country textfields, initialize a `POI` object, and then if-let unwrap the 3 clue textfields and add them to the POI if applicable
17. At the end of the `saveTapped` method, call the delegate method on the delegate property and pass the POI that was just created as an argument
18. In an extension, make the class conform to the `UITextFieldDelegate` protocol
19. In the storyboard, for each textfield in this view, connect the delegate property of the textfield to this class
20. Implement the delegate method `textFieldShouldReturn(_:)`; unwrap the text and make sure it's not empty, then `switch` off the textfield to determine which one called this method; change the `firstResponder` status to the appropriate textfield

#### In `POIsTableViewController.swift`:

21. In an extension, make the class conform to the `AddPOIDelegate` protocol
22. Implement the delegate method `poiWasCreated(_:)`
23. Append the poi that was passed into the method to your array
24. Dismiss the view so the modal animates offscreen
25. Reload the tableview's data
26. In the main class definition, implement the `prepare(for:sender:)` method (it might be commented out)
27. Check the identifier of the segue first; look for the identifier `AddPOIModalSegue`
28. If that identifier is found, use the segue object's `destination` property to grab the view controller we're transitioning to and cast it as an instance of `AddPOIViewController`; you'll need to unwrap this as the cast may not succeed
29. If the unwrap is successful, use the view controller you unwrapped, set its `delegate` property to `self`; this ensures the `POIsTableViewController` will act on behalf of the `AddPOIViewController`

#### In `POIDetailViewController.swift`:

30. Declare 3 `IBOutlet` properties: `locationLabel`, `countryLabel`, and `cluesTextView` using `UILabel` and `UITextView` for the types; wire them to their appropriate subviews in the storyboard
31. Declare a variable property called `poi` of type `POI`
32. Create a private method called `updateViews` that takes no arguments
33. Call the above method inside `viewDidLoad`
34. In `updateViews`, unwrap the poi property with `guard` and set the various model properties to the text of the labels and the textview; you'll have to do a little formatting to show the clues as a list in the `cluesTextView`

#### In `POIsTableViewController.swift`:

35. In the `prepare(for:sender:)` method, add an `else-if` to your existing code to check for a different segue identifier, `ShowPOIDetailSegue`
36. If that identifier is found, unwrap the selected row's index path from the tableview, and then grab the segue's destination view controller and cast it as an instance of `POIDetailViewController` (do these two steps together in a compound `if-let`)
37. If those unwraps are successful, set the detail view controller's `poi` property to the appropriate `POI` from the array you use in this class to track all POIs

### Testing

At this point, the app should run and allow new POIs to be entered. They should appear in the main tableview and when tapped, the app should transition to a detail view to show the rest of the information about each POI.