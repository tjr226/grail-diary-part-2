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

DONE 1. Create a new file using the Cocoa Touch Class template; call it `AddPOIViewController` and make it a subclass of `UIViewController`
DONE 2. Create one more Cocoa Touch Class in the same way called `POIDetailViewController` (also subclassed from `UIViewController`).
DONE 3. Create another Cocoa Touch Class subclassed from `UITableViewCell` called `POITableViewCell`
DONE 4. Create a Swift file called `POI` (this is your model)

### Storyboard Tasks

DONE 5. Set the custom class for each view controller scene in the storyboard to match the names of the files you just created

### Code Tasks

#### In `POI.swift`:

DONE 6. Create a `struct` called `POI` that models 3 properties: `location`, `country`, and `clues`; the data types are `String`, `String`, and an array of `String` objects

#### In `POIsTableViewController.swift`:
(named as POIsViewController in my implementation)

DONE 7. Create an array property to store your `POI` models
DONE 8. Create an `IBOutlet` to link the table view to your code; wire up this outlet to the table view in the storyboard. This is necessary because we are using a table view in a normal `UIViewController`.
DONE? 9. In an extension, make this class conform to the `UITableViewDataSource` protocol
DONE? 10. Implement the following protocol methods: `tableView(_:numberOfRowsInSection:)` and `tableView(_:cellForRowAt:indexPath:)`
DONE? 11. Set the view controller as the delegate and data source for the table view.

#### In `AddPOIViewController.swift`:

DONE 12. Declare `IBOutlet` properties for the 5 textfields in this view; wire these up to their appropriate textfields in the storyboard
DONE 13. Declare a protocol above this class in the same file called `AddPOIDelegate`; declare a single function requirement called `poiWasAdded(_ poi: POI)`
DONE 14. Declare a variable property called `delegate` of type `AddPOIDelegate` and make it optional
DONE 15. Create 2 `IBAction` methods: `cancelTapped(_ sender: UIBarButtonItem)`, and `saveTapped(_ sender: UIBarButtonItem)`; wire them up to their appropriate bar button items in the storyboard
DONE 16. Inside `cancelTapped`, simply dismiss this view as the user has indicated they want to leave this screen
DONE 17. Inside `saveTapped`, guard unwrap the text inside the location and country textfields, initialize a `POI` object, and then if-let unwrap the 3 clue textfields and add them to the POI if applicable
DONE 18. At the end of the `saveTapped` method, call the delegate method on the delegate property and pass the POI that was just created as an argument
DONE 19. In an extension, make the class conform to the `UITextFieldDelegate` protocol

NOT DONE ??? feel like this was skipped in the guided project
20. In the storyboard, for each textfield in this view, connect the delegate property of the textfield to this class

DONE21. Implement the delegate method `textFieldShouldReturn(_:)`; unwrap the text and make sure it's not empty, then `switch` off the textfield to determine which one called this method; change the `firstResponder` status to the appropriate textfield

#### In `POIsTableViewController.swift`:

?? might be done 22. In an extension, make the class conform to the `AddPOIDelegate` protocol
DONE 23. Implement the delegate method `poiWasCreated(_:)`
DONE 24. Append the poi that was passed into the method to your array
DONE 25. Dismiss the view so the modal animates offscreen
DONE 26. Reload the tableview's data
DONE 27. In the main class definition, implement the `prepare(for:sender:)` method (it might be commented out)
DONE 28. Check the identifier of the segue first; look for the identifier `AddPOIModalSegue`
DONE 29. If that identifier is found, use the segue object's `destination` property to grab the view controller we're transitioning to and cast it as an instance of `AddPOIViewController`; you'll need to unwrap this as the cast may not succeed
DONE 30. If the unwrap is successful, use the view controller you unwrapped, set its `delegate` property to `self`; this ensures the `POIsTableViewController` will act on behalf of the `AddPOIViewController`

(also did 28-30 for ShowPOIDetailSegue)

#### In `POITableViewCell.swift`:

DONE 31. Declare `IBOutlet` properties for the following cell elements: `locationLabel`, `countryLabel`, and `cluesCountLabel`; connect them to their appropriate labels in the storyboard
DONE 32. Declare an optional property called `poi` of type `POI`; add a `didSet` observer that calls a function named `updateViews`
DONE 33. Define a private function named `updateViews`; inside, unwrap `poi` and use its properties to populate the various labels with data

#### In `POIDetailViewController.swift`:

DONE 34. Declare 3 `IBOutlet` properties: `locationLabel`, `countryLabel`, and `cluesTextView` using `UILabel` and `UITextView` for the types; wire them to their appropriate subviews in the storyboard
DONE 35. Declare a variable property called `poi` of type `POI`
DONE 36. Create a private method called `updateViews` that takes no arguments
DONE 37. Call the above method inside `viewDidLoad`
DONE 38. In `updateViews`, unwrap the poi property with `guard` and set the various model properties to the text of the labels and the textview; you'll have to do a little formatting to show the clues as a list in the `cluesTextView`

#### In `POIsTableViewController.swift`:

DONE 39. In the `prepare(for:sender:)` method, add an `else-if` to your existing code to check for a different segue identifier, `ShowPOIDetailSegue`
DONE 40. If that identifier is found, unwrap the selected row's index path from the tableview, and then grab the segue's destination view controller and cast it as an instance of `POIDetailViewController` (do these two steps together in a compound `if-let`)
DONE 41. If those unwraps are successful, set the detail view controller's `poi` property to the appropriate `POI` from the array you use in this class to track all POIs

### Testing

At this point, the app should run and allow new POIs to be entered. They should appear in the main tableview and when tapped, the app should transition to a detail view to show the rest of the information about each POI.
