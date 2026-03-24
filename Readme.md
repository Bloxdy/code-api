# Readme
> [!NOTE]
> This is not the official documentation and is not edited or run by any of Bloxd.io's Devs, this is ment to be an additional resource to the official documentation.
 
> [!WARNING]
> This repository may contain out-of-date or incorrect information at times, but the maintaners will try to keep this as up-to-date and correct as they can.

> [!TIP]
> Find information that you think should be added or changed? Create a pull request!

You can run JavaScript when right clicking code blocks and press to code boards.
Editing, by default, is only available to owners and co-owners of worlds lobbies.
The JavaScript can interact with the Bloxd.io game api.

Please use [Bloxd.io's official discord server](https://discord.gg/playbloxd) to report any issues you come across or features you'd like to see added.
Please create a pull request if you notice incorrect, or missing information in this version of the documenation.

## Code Blocks

- Lobby Owners and Co-Owners can find these by searching in the creative menu
- No need to add `press to code`, this text is only needed for code boards, and will automatically be removed from `Code Blocks`
- If you want to run code without opening the code editor, you can trigger the code block by right clicking an adjacent `press to code` board instead, or left clicking (be careful not to break the block).
- Code Blocks can not use `callbacks` because Code Blocks only run when pressed and World Code uses a copy of the function so modifying the functions does nothing.

## World Code

- World owners can access the `World Code` editor by pressing F8 or by clicking the World Code button in the Code Block editor
- Like Code Blocks there is no need to add `press to code` in the `World Code`
- Unlike Code Blocks, World Code can contain `callbacks` and will run the code inside of a callback when the callback is triggered
- There is no way to trigger a Code Block or sign from World Code directly, but their block data can be `eval`ed achieving the same effect.

## Boards

- You can begin a board with `press to code` to run javascript when you right click it.
- Normally you can't edit a code board after placing it, but you can currently work around this by putting a space before `press to code`.
- Boards only allow for a small amount of text, we recommend you use Code Blocks instead, or you can work around this by using multiple boards

## Notes

- Global variables `myId` and `playerId` store the player ID of who is running the code, it can not be used in World code.
- Global variable `thisPos` stores the position of the currently executing code block or press to code board, it can not be used in World code.
- `myId`, `playerId`, `thisPos` and `ownerDbId` are all defined on `api`
- You can use `api.log` or `console.log` for printing and debugging (they do the same thing).
- You can use `Date.now()` or `api.now()`, both return the time in milliseconds.
- Comments like `/* comment */` and `// comment` work.
- World Code and Code Blocks are limited to 16,000 charecters including spaces and linebreaks. The code editor is 80 charecters wide (or 79 when scroll bar shows up).
- The JavaScript `var` variable can be called from World Code, Code Blocks, and Boards, regardless of which ones it is defined in. (Note though that if it is placed in a Code Block or Board that it must be executed before the variable can be used.)

> [!NOTE]
> The official documentation will say that `// comments` will not work, but that is incorrect as they now work.
  
## About this repository
This was created because the original documentation was not well explained or organized and lacked information that many people agreed would be helpful. So here it is, it's not perfect, but it contains more information then the original documentation. There may from time to time be errors in the documenation and it may lack newer information. That's why it's highly recomended that if you notice that there's missing information or that there's an error somewhere that you create a pull request. Also if you think that you have information that may be helpful to new coder that you create a pull request to add it here under the `Information` header. You will find that most of the files here are not formated the same as the original documentation, this is on purpose so that it's easier to search through for the information you want.

## Information
> [!NOTE]
> This section needs more information, please create a pull request if you have information for this section.

## FAQ
**Q:** What is the `playerId` in functions and callbacks?  
**A:** It is field that contains the Id of a player. It is a string containing a negative integer, just like other entities like mobs and items, which also have a string id as a negative integer. `Code Blocks` and `Press to Code` signs can contain the variable `myId` which will hold the id of the player who activated the code. In `World Code`, however, `myId` just holds the value `null` because code gets activated by callbacks, and there is no specific person who triggered the code. However, some callbacks pass to you a `playerId` as a parameter, which is the id of the player that the callback is relevant to. For example, the first argument that `onPlayerChat` passes is `playerId`, which hold a string which is the id of a player.  

**Q:** Where do I use callbacks and why not code blocks instead? 
**A:** You can only use callbacks in `World Code` because `Code Blocks` and `Press to Code` signs only get activated when a player clicks them. Even if you execute the code from the `Code Blocks` or `Press to Code` board, it will not work as a callback, because `World Code` only checks for the callbacks that existed when `World Code` was created, and it essentially saves a copy of them when `World Code` is ran (which happens when the first player joins or `World Code` is updated), so further modification does nothing until World Code is updated.

**Q:** What is the difference between `World Code` and `Code Blocks`?  
**A:** `World Code` is run when a callback in `World Code` gets triggered, upon which it runs the code in that callback. `Code Blocks`, however, only get triggered when a player clicks them or a sign next to them gets triggered. `World Code` is limited to `16,000` characters (including spaces) and you can only have one `World Code` for each world. While `Code Blocks` are also limited to `16,000` characters, you can have an unlimited amount of them, and use them in conjunction with `api.getBlockData` to get practically infinite `World Code`.
