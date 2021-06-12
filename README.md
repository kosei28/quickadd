## QuickAdd
Quickly add new pages or content to your vault.
### Demo
![zApIWkHrKP](https://user-images.githubusercontent.com/29108628/121762835-bb8b2e80-cb38-11eb-8ef6-b65700526caf.gif)

### Getting started
The first thing you'll want to do is add a new choice. A choice can be one of four types.

#### Template
You first need to specify a _template path_. This is a path to the template you wish to insert.

The remaining settings are useful, but optional. You can specify a format for the file name, which is based on the format syntax - which you can see further down this page.
Basically, this allows you to have dynamic file names. If you wrote `{ {{DATE}} {{NAME}}`, it would translate to a file name like `{ 2021-06-12 FileName`, where `FileName` is a value you enter.

You can specify as many folders as you want. If you don't, it'll just create the file in the root directory. If you specify one folder, it'll create the file in there.
If you specify multiple folders, you'll be asked which folder you wish to create the file in when you are creating it.

_Append link_ appends a link to the created file in the file you're currently in.

_Increment file name_ will, if a file with that name already exists, increment the file name. So if a file called `untitled` already exists, the new file will be called `untitled1`.

_Open_ will open the file you've created. By default, it opens in the active pane. If you enable _New tab_, it'll open in a new tab in the direction you specified.
![image](https://user-images.githubusercontent.com/29108628/121773888-3f680980-cb7f-11eb-919b-97d56ef9268e.png)

#### Capture
_Capture To_ is the name of the file you are capturing to. This also supports the format syntax, which allows you to use dynamic file names.
I have one for my daily journal with the name `bins/daily/{{DATE:gggg-MM-DD - ddd MMM D}}.md`. This automatically finds the file for the day, and whatever I enter will be captured to it.

_Prepend_ will put whatever you enter at the bottom of the file.
_Task_ will format it as a task.
_Append link_ will append a link to the file you have open in the file you're capturing to. 
_Insert after_ will allow you to insert the text after some line with the specified text. I use this in my journal capture, where I insert after the line `## What did I do today?`.

_Capture format_ lets you specify the exact format that you want what you're capturing to be inserted as. You can do practically anything here. Think of it as a mini template.
See the format syntax further down on this page for inspiration.
In my journal capture, I have it set to `- {{DATE:HH:mm}} {{VALUE}}`. This inserts a bullet point with the time in hour:minute format, followed by whatever I entered in the prompt.

![image](https://user-images.githubusercontent.com/29108628/121774039-4d6a5a00-cb80-11eb-89be-0aceefaa658b.png)

#### Macro
Macros are powerful tools that allow you to execute any sequence of Obsidian commands and user scripts.
User scripts are Javascript scripts that you can write to do something in Obsidian.

Each _macro choice_ has an associated _macro_. A macro choice allows you to activate a macro from the QuickAdd suggester.

This is what the settings for a _macro choice_ looks like.

![image](https://user-images.githubusercontent.com/29108628/121774145-22ccd100-cb81-11eb-8873-7533755bdf32.png)

Now, you can have any amount of _macros_. You can use the macro manager to... manage them.
If you have any macros that you want to run on plugin load - which is often just when you start Obsidian - you can specify that here, too.

This allows you to, for example, create a daily note automatically when you open Obsidian.

![image](https://user-images.githubusercontent.com/29108628/121774198-81924a80-cb81-11eb-9f80-9816263e4b6f.png)

This is what my `logBook` macro looks like. It's pretty plain - it just executes one of my user scripts.

![image](https://user-images.githubusercontent.com/29108628/121774245-cfa74e00-cb81-11eb-9977-3ddac04dc8bd.png)

The `logBook` user script simply updates the book in my daily page to something I specify in a prompt.
Here it is - with some comments that explain the code.

```js
// You have to export the function you wish to run.
// QuickAdd automatically passes a parameter, which is an object with the Obsidian app object
// and the QuickAdd API (see description further on this page).
module.exports = async (params) => {
    // Object destructuring. We pull inputPrompt out of the QuickAdd API in params.
    const {quickAddApi: {inputPrompt}} = params;
    // Here, I pull in the update function from the MetaEdit API.
    const {update} = app.plugins.plugins["metaedit"].api;
    // This opens a prompt with the header "📖 Book Name". val will be whatever you enter.
    const val = await inputPrompt("📖 Book Name");
    // This gets the current date in the specified format.
    const date = window.moment().format("gggg-MM-DD - ddd MMM D");
    // Invoke the MetaEdit update function on the Book property in my daily journal note.
    // It updates the value of Book to the value entered (val).
    await update('Book', val, `bins/daily/${date}.md`)
}
```

#### Multi
Multi-choices are pretty simple. They're like folders for other choices. Here are mine. They're the ones which you can 'open' and 'close'.

![image](https://user-images.githubusercontent.com/29108628/121774481-e39f7f80-cb82-11eb-92bf-6d265529ba06.png)
