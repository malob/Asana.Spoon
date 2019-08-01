# Asana.spoon

A Simple [Hammerspoon](http://www.hammerspoon.org) Spoon that creates a new task in Asana with a given name in a given workspace.

Example configuration (using [SpoonInstall.spoon](http://www.hammerspoon.org/Spoons/SpoonInstall.html) and [Seal.spoon](http://www.hammerspoon.org/Spoons/Seal.html)):
```lua
-- Load configuration constants
consts = require "configConsts" -- just a table

-- Load Asana spoon and configure it with API key
spoon.SpoonInstall:andUse("Asana", { config = { apiKey = consts.asanaApiKey } })

-- Load Seal and setup user actions to add task to Asana workspaces
spoon.SpoonInstall:andUse(
  "Seal",
  {
    fn = function(x)
      x:loadPlugins({"apps", "calc", "useractions", "rot13"})
      x.plugins.useractions.actions = {
        ["New Asana task in " .. consts.asanaWorkWorkspaceName] = {
          fn      = function(y) spoon.Asana:createTask(y, consts.asanaWorkWorkspaceName) end,
          keyword = "awork"
        },
        ["New Asana task in " .. consts.asanaPersonalWorkspaceName] = {
          fn      = function(y) spoon.Asana:createTask(y, consts.asanaPersonalWorkspaceName) end,
          keyword = "ahome"
        }
      }
      x:refreshAllCommands()
    end,
    start = true,
    hotkeys = { toggle = { "cmd", "space" } }
  }
)
```

With this setup adding a new task to Asana is as easy pressing `Command + Space` to launch Seal and entering, e.g., "awork Do that thing I forgot to do".
