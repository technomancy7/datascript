# Infoscript
A small parsing language designed to be extendible and flexible, useful for a variety of purposes.
I made it because I needed a human-readable and flexible way of handling data for multiple projects, and wanted to see if I could make one library that would work for them all, partially to test myself, partially because I thought it'd be fun, and partially because I could then re-use data for different projects without needing different formats.

## TODO
- [ ] Add some flexibility and options for creation
    - [ ] Disable risky features like eval unless chosen to enable
- [ ] Add a way for @ and + operators and set commands to define a datatype, such as numbers being set as int
- [ ] More default commands to allow more flexibility
- [ ] An AIML-style extension for creating chat bots
- [ ] A Discord extension to integrate with personal projects
- [ ] Other things I can't remember right now.

## Example
```
// Lines starting with @ define variables
// Here we defined the script author, a metavariable that doesn't do anything but can be accessed later.
@author Kaiser

// Defining a pointless variable to highlight some flexibility later.
@lsname admins

// Using backticks evals the line. This creates a python list variable, same as it would in raw py.
@admins `[]`

//Here we're doing two fancy things.
//Line starting with + works like @ does for SETTING, but instead attempts to append the value to the
// variable, useful for lists.
//Variable name here is using the inline evaluation syntax, @{variable_name}, which swaps in the value
// of that variable, in this case, admins.
//This is not very useful in this example, but it highlights how the inline evaluation works.
+@{lsname} Kai

// Code can also be split in to blocks.
// We give this block the name `first_trigger`. This will be useful in a moment.
start first_trigger
// We are defining variables like usual here, but instead of being added to the global variables for
// the engine, they get added to a sub-dictionary unique this block.
// This is useful for keeping things seperated, or to have an abstraction of objects.
@trigger This is my first trigger
+responses This is a first response
+responses This is a second response
end

// If we don't give a block a name, it gets assigned a default one.
// It works the same, but makes it harder to access later.
start
@trigger Trigger 2

// Lines starting with # are commands. Infoscript has a few built-in commands, but the engine
// can be easily extended to allow it to be used for a variety of purposes.
#echo The second response in the first trigger was @{first_trigger.responses#1}!

// Inline execution works like inline evaluation, but runs a command, and inserts the result of the command.
#echo @{author} #{get: author} #{test} #{test: or did we}

#append ls Borks
#append ls No borks?
#append ls @{author}
end


#exec _t = "We can inline eval too!"
#exec print(_t)
```

```
Output:
The second response in the first trigger was This is a second response!
Kaiser Kaiser TEST:success!!! TEST:or did we!!!
We can inline eval too!

Variables post execution: 
{'_number_of_blocks': 2, '_version': '0test', '_current_block': '', 'author': 'Kaiser', 'revision': 1, 'lsname': 'admins', 'admins': ['Kai'], 'first_trigger': {'trigger': 'This is my first trigger', 'responses': ['This is a first response', 'This is a second response']}, 'block_1': {'trigger': 'Trigger 2'}, 'ls': ['Borks', 'No borks?', 'Kaiser']}
```
