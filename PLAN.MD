# PDF-iler
Lets take existing PDFs and use them in a visual environment to build templates for reading other similar PDFs so we can parse a whole bunch of them and store the data for normal use.

## Functional Reqs
This app will have two key functions, the first is the Template Builder which allows us to load existing PDFs and build/save templates. The second part lets us specify a template and a bunch of PDFs to parse, our app goes through the data, reads what it's asked to and spits out a structured file with the requested data.
### Template Builder
    1. Visual interface for loading PDF files
    2. Mouse interaction for creating target check areas
        a. General area to check box (footer, grand totals box, etc)
        b. specific target to look for (data label "Grand Total", "Subtotal", "Amount Due" etc)
        c. expected area for target data (Actual numbers/strings we want to pull into our data)
        d. ability to set multiple target groups (general box + target label + target data)
    3. Method to identify what data our targets hold so we can file it correctly in our output
    4. Ability to save template for future use
### Parser
    1. Interface to allow selecting of an existing template and providing target for parsing (file or folder)
    2. Parse files using the template and create a new file with the parsed data
    3. Save new file

## Data Validation
We'll need some method of confirming out data is correct. I think having two explicit but very independent methods could work.
### Confidence Value
Based on how we visually parse the data I suspect we'll be able to determine some measurable value for how confident our app is in it's accuracy. Attaching this value to our data set can give us a guide on where to look first for errors and should provide a feedback loop for improving functionality.
### Random Samples
As with and system doing things in builk we should be flagging at random some subset of our data for manual validation by the user. While not a perfect method this should help illustrate if things are working as expected. Perhaps we use a user-confidence modifier that decreases the size of the subset as it's value increases. This could be either manually adjusted by the user or automatically adjusted if we build a comparitor into the app.

## Stuff to Consider