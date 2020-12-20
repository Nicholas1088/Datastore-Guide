## What is Lua? Lua is a basic coding language implemented into roblox, it made roblox and you can use it for free at Roblox Studio! It's to teach everyone how to code in a different language maybe they're never seen before, they also let you access their API through Datastores, (not really "access" just allow you to save data to it!)

## Hello everyone, today i’ll be going over the basic foundation of Datastores along with some key notes you should know about them. We’ll break this down into a couple of sections and i’ll let you know what each and everything does except for the basic starting stuff such as game.Players.PlayerAdded etc. Anyway lets get to work!!! 


## Alrighty, so first off you’re going to want to start off with an event such as PlayerAdded and PlayerRemoving so lets do that below (full code will be at the bottom i’ll be piecing them together and forming them as we go so make sure to read the bottom along with the notes for each and everything.) First off we gotta make our DataStore service!
		

local DatastoreService = game:GetService("DataStoreService")
local DS = DatastoreService:GetDataStore("MyData") --Name this anything. Even with spaces.


```lua
game.Players.PlayerAdded:Connect(function(player)
```
end)

## We can copy and paste the player added and change some stuff around to say Removing so lets do that.
```lua
game.Players.PlayerRemoving:Connect(function(player)

end)
```
		
## After we have our events we’re gonna start with a basic rundown of leaderstats. You might know what these are already and that’s amazing!

## So after we’ve gotten our Events what you wanna do is make a leaderstats folder and value inside of that player ADDED not REMOVING.
```lua
game.Players.PlayerAdded:Connect(function(player)
	local leaderstats = Instance.new(“Folder”) 
	-- Don’t put , player, I'll explain why later.
	leaderstats.Parent = player -- Do this instead
	leaderstats.Name = "leaderstats" -- Name of the folder

	local Coins = Instance.new( “IntValue”)
	-- Once again don’t put , leaderstats I promise i’ll explain.

	Coins.Parent = leaderstats
	Coins.Name = "Coins	"
	Coins.Value = 0 -- Put the value you want them to start with.

	-- After you're done with that you want to go ahead and make another variable. Call this whatever but do not add ANY CODE TO IT AFTER

	local data -- Just do this for the data because we need to be able to call it later (calling is when you make a variable with no = and then use it later such as data = DS:GetAsync(key)

	-- Now we need a key, a key in LUA is just a foundation of the game trying to find something or anything relative to our int value, such as Coins we would do for a key "coins_" now this is needed because a key is a specific way of storing data. 

	local key = "coins_"..player.UserId -- This is our key, the key is your intValue name so if you had Diamonds you would use "diamonds_"..player.UserId, the player.UserId is the player's specific id because if they change their name their data could get lost and we don't want that.

	-- Now we have to make our Pcall, a pcall in roblox is something that is used for catching errors and making sure no data gets lost, we do this by simply adding a success errorMessage.
	local success, errorMessage = pcall(function()
		-- Now once we're done here we need to add some code into it.
		-- We have to call our data store now. Also that data.
		data = DS:GetAsync(key)
	end)

	if success then
		data = Coins.Value -- Setting the data to the Coins.Value because now the data is getting the key which is our player key.
		print("Got Value") -- Printing for safety and checking!
	else
		print(errorMessage) --Prints any errorMessage the game might not spew out.
		-- Now this is the end of our player Added Event!
	end
end)

game.Players.PlayerRemoving:Connect(function(player)
	-- We're basically going to do the same thing but switch up a few words.
	-- We don't need data anymore.
	-- But we do need our key!

	local key = "coins_"..player.UserId

	local success, errorMessage = pcall(function()
		DS:SetAsync(key, player.leaderstats,Coins.Value) -- Sets the value to save. (put the name of the value where we put the Coins.Name = "Coins" so if you change "Coins" to "Gold" make sure it's .Gold.Value
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

	for i, v in pairs(game.Players:GetPlayer()) do

		local key = "coins_"..v.UserId -- We gotta use V because we don't have player.
		-- This is a for loop more information will be in the other beginner resources!

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
end


-- Almost forgot, you know that , player we were talking about earlier? Yeah you don't want to use that becuase it is deprecated, it would look something like this 

local Coins = Instance.new("IntValue", leaderstats) -- This works the same with player and it's deprecated so using it is not the best option.

-- Deprecated means that it should not be used or it's not recommended to be used in programming.

-- Now our data store is complete!

```
