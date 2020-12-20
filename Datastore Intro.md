# Basic Guide to Datastores

Hello everyone, today I will be going over the basic foundation of Datastores along with some key information you should know. We’ll break this down into a couple of sections and I’ll let you know what each and everything does. Let's get to work!!! 

Before we do anything, we gotta make our Datastore Service!
		
```lua
local DatastoreService = game:GetService("DataStoreService")
local DS = DatastoreService:GetDataStore("MyData") --Name this anything. Even with spaces.
```
First off, you’re going to want to start off with an event known as PlayerAdded and PlayerRemoving. Let's do that below. 

```lua
game.Players.PlayerAdded:Connect(function(player)

end)

game.Players.PlayerRemoving:Connect(function(player)

end)
```

After you’ve put in your PlayerAdded and PlayerRemoving events, what you wanna do is make a leaderstats folder and value inside of that player ADDED not REMOVING.

```lua
game.Players.PlayerAdded:Connect(function(player)
	local leaderstats = Instance.new("Folder") 
	-- Don’t put ("Folder", player), I'll explain why later.
	leaderstats.Parent = player -- Do this instead
	leaderstats.Name = "leaderstats" -- Name of the folder

	local Coins = Instance.new("IntValue")
	-- Once again don’t put ("IntValue", leaderstats), I promise I’ll explain.

	Coins.Parent = leaderstats
	Coins.Name = "Coins"
	Coins.Value = 0 -- Put the value you want them to start with.

	-- After you're done with that you want to go ahead and make another variable. Call this whatever but do not add ANY CODE TO IT AFTER.

	local data -- Just do this for the data because we need to be able to call it later (calling is when you make a variable with no = and then use it later such as data = DS:GetAsync(key)

	-- Now we need a key, a key in Datstores is a string used to distinguish which data to access


	local key = "coins_"..player.UserId -- This is our key, the key in this case is our IntValue name so if you had Diamonds you would use "diamonds_"..player.UserId, the player.UserId is the player's specific id because if they change their name their data could get lost and we don't want that.

	-- Now we have to make our Pcall, a pcall in roblox is something that is used for catching errors and making sure no data gets lost, we do this by simply adding a success and errorMessage variable.
	local success, errorMessage = pcall(function()
        -- Now once we're done here we need to get the data from our datastore.
        
		data = DS:GetAsync(key)
	end)

	if success then
		Coins.Value = data -- Setting the IntValue's value to the data we've retrieved if the pcall went well.
		print("Got Value") -- Printing for safety and checking!
	else
		print(errorMessage) --Prints any errors that might have happened if the pcall did not go well.
		-- Now this is the end of our PlayerAdded event!
	end
end)

game.Players.PlayerRemoving:Connect(function(player)
	-- We're basically going to do the same thing but switch up a few words.

	local key = "coins_"..player.UserId

	local success, errorMessage = pcall(function()
		DS:SetAsync(key, player.leaderstats.Coins.Value) -- Sets the value to save. (put the name of the value where we put the Coins.Name = "Coins" so if you change "Coins" to "Gold" make sure it's .Gold.Value
	end)

	if success then
		print("Saved!")
	else
		print(errorMessage)
	end
end)

-- Now we need a BindToClose!

game:BindToClose(function() 
	-- A BindToClose does not use a player argument, a BindToClose activates when the game shuts down or the game crashes causing data to save!
	-- Because it cannot access player we need a for loop.

	for _, player in pairs(game.Players:GetPlayers()) do
        -- This is a for loop more information will be in the other beginner resources!
        local key = "coins_"..player.UserId
        
		-- Now we can save our data with DS

		local success, errorMes = pcall(function()
			DS:SetAsync(key, player.leaderstats.Coins.Value) -- Same thing as above with the Player Removing.
		end)

		if success then
			print("Saved on close") -- You probably wont see this print
		else
			print(errorMes) -- Same with this.
			
		end
	end
end)

-- Before we finish, you know that ("Folder", player) we were talking about earlier? You don't want to use that becuase the second argument, representing the parent is deprecated, it would look something like this:

local Coins = Instance.new("IntValue", leaderstats) -- This is the same as using the parent property, however, because it is deprecated, you don't want to use it. It could end up not working one day.

-- Deprecated means that it should not be used or it's not recommended to be used in programming.

-- Now our datastore is complete!

```


```lua

-- Full Code Below:

local DatastoreService = game:GetService("DataStoreService")
local DS = DatastoreService:GetDataStore("MyData") 

game.Players.PlayerAdded:Connect(function(player)
	local leaderstats = Instance.new("Folder") 
	leaderstats.Parent = player 
	leaderstats.Name = "leaderstats" 

	local Coins = Instance.new("IntValue")

	Coins.Parent = leaderstats
	Coins.Name = "Coins	"
	Coins.Value = 0 

	local data 

	local key = "coins_"..player.UserId 

	
	local success, errorMessage = pcall(function()
	
		data = DS:GetAsync(key)
	end)

	if success then
		data = Coins.Value
		print("Got Value") 
	else
		print(errorMessage)
	end
end)

game.Players.PlayerRemoving:Connect(function(player)

	local key = "coins_"..player.UserId

	local success, errorMessage = pcall(function()
		DS:SetAsync(key, player.leaderstats.Coins.Value) 
	end)

	if success then
		print("Saved!")
	else
		print(errorMessage)
	end
end)


game:BindToClose(function() 
	for i, v in pairs(game.Players:GetPlayers()) do
		local key = "coins_"..v.UserId 
		
		local success, errorMes = pcall(function()
		    DS:SetAsync(key, v.leaderstats.Coins.Value) 
		end)
        if success then
	        print("Saved on close") 
	    else
            print(errorMes) 
	    end
     end
end)
```
