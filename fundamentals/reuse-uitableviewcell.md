# Reuse UITableViewCell

Link: [https://stackoverflow.com/a/16330118](https://stackoverflow.com/a/16330118)

The problem: a table view can potentially have thousands of rows (or millions, whatever). It would be tedious and wasteful to create a separate cell for each data row. Instead, the table view only asks for as many rows as it displays on the screen at the same time (this is typically no more than 10-15-20 cells). This is manageable, doesn't consume a lot of memory and plays nice with the fact that not all of the rows are visible on the display anyway.

So, when the table view needs a new cell to be displayed (because the user has scrolled the view), it takes a cell that went out of the visible area, and queues it back to the end, reusing it
