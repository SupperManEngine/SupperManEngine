local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "SlapBattlesSolo",
   LoadingTitle = "Insanitgo",
loadingSubtitle = "by Solo",
 Theme = "Green", -- DarkBlue, Green, Light, Default - more coming soon!  
        
        DisableRayfieldPrompts = false,

        ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },
   Discord = {
      Enabled = false,
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },
   KeySystem = true, -- Set this to true to use our key system
   KeySettings = {
      Title = "SlapBattlesSolo",
      Subtitle = "Made By Solo",
      Note = "GO To My Yt Channel: ScripterWinner",
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"SlapBattlesDaBest"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }


        
})

local MainTab = Window:CreateTab("Main", nil) -- Title, Image
Rayfield:Notify({
   Title = "Enjoy",
   Content = "Insane",
   Duration = 3,
   Image = 4483362458,
   Actions = { -- Notification Buttons

      Ignore = { -- Duplicate this table (or remove it) to add and remove buttons to the notification.
         Name = "Solo",
         Callback = function()
            print("The user tapped Okay!")
         end
      },

},
})

local Button = MainTab:CreateButton({
   Name = "Slap Aura (Hit One Person at A Time Or U will be KICKED (PUBLIC ONLY!!))",
   Callback = function()
       --[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
function isSpawned(player)
   if workspace:FindFirstChild(player.Name) and player.Character:FindFirstChild("HumanoidRootPart") then
       return true
   else
       return false
   end
end

while wait() do
   for i, v in pairs(game.Players:GetPlayers()) do
       if isSpawned(v) and v ~= game.Players.LocalPlayer and not v.Character.Head:FindFirstChild("UnoReverseCard") then
           if (v.Character.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 50 then
               game:GetService("ReplicatedStorage").b:FireServer(v.Character["Right Arm"])
               wait(0.1)
           end
       end
   end
end        
   end,
})

local arenaY = -50  -- Set the Y position of the arena floor (adjust as needed)
local platformSize = Vector3.new(1000, 7, 1000)  -- Slightly taller platform (height changed from 5 to 7)

local isAntiVoidActive = false  -- Boolean to track if the anti-void mechanism is active or not
local antiVoidCoroutine = nil  -- Reference to the coroutine

-- Create the large platform at the same height as the arena but slightly raised
local function createArenaPlatform()
    -- Create the part that will act as the platform
    local platform = Instance.new("Part")
    platform.Size = platformSize  -- Size of the platform (increased height to 7 for a little more thickness)
    platform.Position = Vector3.new(0, arenaY + 35, 0)  -- Raise platform slightly above the arena by 7
    platform.Anchored = true  -- Anchor the platform so it doesn't move
    platform.CanCollide = true  -- Make the platform collidable so players can walk on it
    platform.BrickColor = BrickColor.new("Bright blue")  -- Color of the platform (change if needed)
    platform.Parent = game.Workspace  -- Add the platform to the game workspace
end

-- Function to prevent players from falling into the void by ensuring they stand on the platform
local function preventFallingIntoVoid()
    local player = game.Players.LocalPlayer  -- Get the local player
    
    -- Wait for the character to load
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    
    -- Create the large platform at the arena height (slightly raised)
    createArenaPlatform()

    -- Keep the player above the platform and prevent falling into the void
    while isAntiVoidActive do  -- This loop will run as long as isAntiVoidActive is true
        local position = humanoidRootPart.Position

        -- If the player falls below the platform Y position, make sure they stand on the platform
        if position.Y < arenaY + 35 then  -- Adjusted to prevent falling below the slightly raised platform
            -- Set the player's Y position to the platform level to stop them from falling
            local newPosition = Vector3.new(position.X, arenaY + 7, position.Z)
            humanoidRootPart.CFrame = CFrame.new(newPosition)
        end
        wait(0.1)  -- Check every 0.1 seconds to keep the player from falling
    end
end

-- Function to start the anti-void system
local function startAntiVoid()
    -- Only start the anti-void if it's not already active
    if not isAntiVoidActive then
        isAntiVoidActive = true
        -- Start the preventFallingIntoVoid function in a coroutine so it runs in the background
        antiVoidCoroutine = coroutine.create(preventFallingIntoVoid)
        coroutine.resume(antiVoidCoroutine)
    end
end

-- Function to stop the anti-void system
local function stopAntiVoid()
    -- Set the flag to false to stop the anti-void system
    isAntiVoidActive = false
    -- If the coroutine is running, stop it
    if antiVoidCoroutine then
        coroutine.close(antiVoidCoroutine)  -- This will safely terminate the coroutine
        antiVoidCoroutine = nil  -- Reset the reference
    end
end

-- Toggle function that will be connected to your UI toggle button
local function toggleAntiVoid(Value)
    if Value then
        -- If the toggle is on, activate the anti-void system
        startAntiVoid()
    else
        -- If the toggle is off, deactivate the anti-void system
        stopAntiVoid()
    end
end

-- Your toggle creation, where the callback connects to the anti-void toggle
local Toggle = MainTab:CreateToggle({
    Name = "Anti-Void Toggle",  -- The name of the toggle in the UI
    CurrentValue = false,  -- Default value (off)
    Flag = "Toggle1",  -- Unique identifier for the toggle (for saving configurations)
    Callback = function(Value)
        toggleAntiVoid(Value)  -- Call the toggleAntiVoid function and pass the value of the toggle
    end,
})


local Dropdown = MainTab:CreateDropdown({
   Name = "Farms",
   Options = {"Bob Farm (20-30 mins u should MUST have enough slaps)","Coming Soon"},
   CurrentOption = {"Option 1"},
   MultipleOptions = false,
   Flag = "Dropdown1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Options)
       
   end,
})

local Button = MainTab:CreateButton({
   Name = "Hitbox",
   Callback = function()
        --[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
-- leave a like pls


_G.HeadSize = 20
_G.Disabled = true
 
game:GetService('RunService').RenderStepped:connect(function()
if _G.Disabled then
for i,v in next, game:GetService('Players'):GetPlayers() do
if v.Name ~= game:GetService('Players').LocalPlayer.Name then
pcall(function()
v.Character.HumanoidRootPart.Size = Vector3.new(_G.HeadSize,_G.HeadSize,_G.HeadSize)
v.Character.HumanoidRootPart.Transparency = 0.7
v.Character.HumanoidRootPart.BrickColor = BrickColor.new("Really blue")
v.Character.HumanoidRootPart.Material = "Neon"
v.Character.HumanoidRootPart.CanCollide = false
end)
end
end
end
end)
   end,
        
})

local Button = MainTab:CreateButton({
   Name = "AntiRagdoll",
   Callback = function()
        -- Anti Knockback Script for Roblox (For use with mycomplier.io)

-- Settings for Knockback Prevention
local preventionDuration = 0.5  -- Time to prevent knockback (seconds)
local maxForce = Vector3.new(10000, 10000, 10000)  -- Max force to block any movement
local velocityZero = Vector3.new(0, 0, 0)  -- Counteract any velocity applied to the character

-- Variables
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")
local isFrozen = false  -- Flag to track if the character is already frozen

-- Function to freeze the character mid-air
local function freezeCharacter()
    if isFrozen then return end  -- Do nothing if already frozen

    isFrozen = true  -- Set the flag to indicate character is frozen

    -- Create a BodyVelocity to counter any knockback force
    local bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = maxForce  -- Allow enough force to prevent movement
    bodyVelocity.Velocity = velocityZero  -- Set velocity to zero to stop movement
    bodyVelocity.P = 10000  -- Increase power to immediately stop velocity
    bodyVelocity.Parent = humanoidRootPart  -- Attach it to the character's root part

    -- Freeze for the duration
    wait(preventionDuration)

    -- After the freeze duration, remove the BodyVelocity to allow movement again
    bodyVelocity:Destroy()

    -- Reset the frozen flag to allow next freeze
    isFrozen = false
end

-- Listen for hits (e.g., slaps or collisions)
humanoidRootPart.Touched:Connect(function(hitPart)
    -- Check if the hit part is from a valid source (e.g., another player or ability)
    -- This condition will ignore the blue hitbox and other irrelevant collisions

    -- Check if the hit part belongs to a slap or a specific part
    if hitPart and hitPart.Parent and hitPart.Parent:FindFirstChild("Humanoid") then
        -- Only freeze if the hit is from a slap (or other specific part)
        freezeCharacter()
    end

    -- Check if the hitPart is part of the blue hitbox and do not freeze in that case
    if hitPart.Name == "BlueHitbox" then  -- Change "BlueHitbox" to your hitbox part's name
        return  -- Don't freeze the character when entering the blue hitbox
    end

    -- Add other conditions for other unwanted hitboxes if needed
end)
          
   end,
})
local MainTab = Window:CreateTab("Local", 4483362458) -- Title, Image
local Slider = MainTab:CreateSlider({
   Name = "Walkspeed (Max 300)",
   Range = {0, 300},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "Slider1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
   end,
})
local Slider = MainTab:CreateSlider({
   Name = "JumpPower (Max 400)",
   Range = {0, 400},
   Increment = 1,
   Suffix = "LOOK HES FLYING",
   CurrentValue = 16,
   Flag = "Slider1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = (Value)
   end,
})

local MainTab = Window:CreateTab("Scripts", 4483362458) -- Title, Image
local Button = MainTab:CreateButton({
   Name = "Kill All OP (NotMadeByMe",
   Callback = function()
       --Script made by r3da09
 
local Targets = {"All"} -- "All", "Target Name", "r
 
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
 
local AllBool = false
 
local GetPlayer = function(Name)
    Name = Name:lower()
    if Name == "all" or Name == "others" then
        AllBool = true
        return
    elseif Name == "random" then
        local GetPlayers = Players:GetPlayers()
        if table.find(GetPlayers,Player) then table.remove(GetPlayers,table.find(GetPlayers,Player)) end
        return GetPlayers[math.random(#GetPlayers)]
    elseif Name ~= "random" and Name ~= "all" and Name ~= "others" then
        for _,x in next, Players:GetPlayers() do
            if x ~= Player then
                if x.Name:lower():match("^"..Name) then
                    return x;
                elseif x.DisplayName:lower():match("^"..Name) then
                    return x;
                end
            end
        end
    else
        return
    end
end
 
local Message = function(_Title, _Text, Time)
    game:GetService("StarterGui"):SetCore("SendNotification", {Title = _Title, Text = _Text, Duration = Time})
end
 
local SkidFling = function(TargetPlayer)
    local Character = Player.Character
    local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
    local RootPart = Humanoid and Humanoid.RootPart
 
    local TCharacter = TargetPlayer.Character
    local THumanoid
    local TRootPart
    local THead
    local Accessory
    local Handle
 
    if TCharacter:FindFirstChildOfClass("Humanoid") then
        THumanoid = TCharacter:FindFirstChildOfClass("Humanoid")
    end
    if THumanoid and THumanoid.RootPart then
        TRootPart = THumanoid.RootPart
    end
    if TCharacter:FindFirstChild("Head") then
        THead = TCharacter.Head
    end
    if TCharacter:FindFirstChildOfClass("Accessory") then
        Accessory = TCharacter:FindFirstChildOfClass("Accessory")
    end
    if Accessoy and Accessory:FindFirstChild("Handle") then
        Handle = Accessory.Handle
    end
 
    if Character and Humanoid and RootPart then
        if RootPart.Velocity.Magnitude < 50 then
            getgenv().OldPos = RootPart.CFrame
        end
        if THumanoid and THumanoid.Sit and not AllBool then
            return Message("Error Occurred", "Targeting is sitting", 5) -- u can remove dis part if u want lol
        end
        if THead then
            workspace.CurrentCamera.CameraSubject = THead
        elseif not THead and Handle then
            workspace.CurrentCamera.CameraSubject = Handle
        elseif THumanoid and TRootPart then
            workspace.CurrentCamera.CameraSubject = THumanoid
        end
        if not TCharacter:FindFirstChildWhichIsA("BasePart") then
            return
        end
 
        local FPos = function(BasePart, Pos, Ang)
            RootPart.CFrame = CFrame.new(BasePart.Position) * Pos * Ang
            Character:SetPrimaryPartCFrame(CFrame.new(BasePart.Position) * Pos * Ang)
            RootPart.Velocity = Vector3.new(9e7, 9e7 * 10, 9e7)
            RootPart.RotVelocity = Vector3.new(9e8, 9e8, 9e8)
        end
 
        local SFBasePart = function(BasePart)
            local TimeToWait = 2
            local Time = tick()
            local Angle = 0
 
            repeat
                if RootPart and THumanoid then
                    if BasePart.Velocity.Magnitude < 50 then
                        Angle = Angle + 100
 
                        FPos(BasePart, CFrame.new(0, 1.5, 0) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle),0 ,0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, -1.5, 0) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(2.25, 1.5, -2.25) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(-2.25, -1.5, 2.25) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, 1.5, 0) + THumanoid.MoveDirection,CFrame.Angles(math.rad(Angle), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, -1.5, 0) + THumanoid.MoveDirection,CFrame.Angles(math.rad(Angle), 0, 0))
                        task.wait()
                    else
                        FPos(BasePart, CFrame.new(0, 1.5, THumanoid.WalkSpeed), CFrame.Angles(math.rad(90), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, -1.5, -THumanoid.WalkSpeed), CFrame.Angles(0, 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, 1.5, THumanoid.WalkSpeed), CFrame.Angles(math.rad(90), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, 1.5, TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(math.rad(90), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, -1.5, -TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(0, 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, 1.5, TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(math.rad(90), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(math.rad(90), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(0, 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, -1.5 ,0), CFrame.Angles(math.rad(-90), 0, 0))
                        task.wait()
 
                        FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(0, 0, 0))
                        task.wait()
                    end
                else
                    break
                end
            until BasePart.Velocity.Magnitude > 500 or BasePart.Parent ~= TargetPlayer.Character or TargetPlayer.Parent ~= Players or not TargetPlayer.Character == TCharacter or THumanoid.Sit or Humanoid.Health <= 0 or tick() > Time + TimeToWait
        end
 
        workspace.FallenPartsDestroyHeight = 0/0
 
        local BV = Instance.new("BodyVelocity")
        BV.Name = "EpixVel"
        BV.Parent = RootPart
        BV.Velocity = Vector3.new(9e8, 9e8, 9e8)
        BV.MaxForce = Vector3.new(1/0, 1/0, 1/0)
 
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
 
        if TRootPart and THead then
            if (TRootPart.CFrame.p - THead.CFrame.p).Magnitude > 5 then
                SFBasePart(THead)
            else
                SFBasePart(TRootPart)
            end
        elseif TRootPart and not THead then
            SFBasePart(TRootPart)
        elseif not TRootPart and THead then
            SFBasePart(THead)
        elseif not TRootPart and not THead and Accessory and Handle then
            SFBasePart(Handle)
        else
            return Message("Error Occurred", "Target is missing everything", 5)
        end
 
        BV:Destroy()
        Humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
        workspace.CurrentCamera.CameraSubject = Humanoid
 
        repeat
            RootPart.CFrame = getgenv().OldPos * CFrame.new(0, .5, 0)
            Character:SetPrimaryPartCFrame(getgenv().OldPos * CFrame.new(0, .5, 0))
            Humanoid:ChangeState("GettingUp")
            table.foreach(Character:GetChildren(), function(_, x)
                if x:IsA("BasePart") then
                    x.Velocity, x.RotVelocity = Vector3.new(), Vector3.new()
                end
            end)
            task.wait()
        until (RootPart.Position - getgenv().OldPos.p).Magnitude < 25
        workspace.FallenPartsDestroyHeight = getgenv().FPDH
    else
        return Message("Error Occurred", "Random error", 5)
    end
end
 
if not Welcome then Message("Script by R3da09", "Enjoy!", 5) end
getgenv().Welcome = true
if Targets[1] then for _,x in next, Targets do GetPlayer(x) end else return end
 
if AllBool then
    for _,x in next, Players:GetPlayers() do
        SkidFling(x)
    end
end
 
for _,x in next, Targets do
    if GetPlayer(x) and GetPlayer(x) ~= Player then
        if GetPlayer(x).UserId ~= 1414978355 then
            local TPlayer = GetPlayer(x)
            if TPlayer then
                SkidFling(TPlayer)
            end
        else
            Message("Error Occurred", "This user is whitelisted! (Owner)", 5)
        end
    elseif not GetPlayer(x) and not AllBool then
        Message("Error Occurred", "Username Invalid", 5)
    end
end
 
   end,
})

local Button = MainTab:CreateButton({
   Name = "Become Diddy (Key: (iammusic) No Capitals  (FUCK ALL))",
   Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/1price/usercreation/refs/heads/main/UserCreation.lua", true))()
   end,
})

local Button = MainTab:CreateButton({
   Name = "Animation FE (Fucking Insane)",
   Callback = function()
       loadstring(game:HttpGet(('https://pastebin.com/raw/1p6xnBNf'),true))()
   end,
})

local Button = MainTab:CreateButton({
   Name = "Inf Yield (NotMadeByMe)",
   Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
   end,
})

local Button = MainTab:CreateButton({
   Name = "Giang Hub SlapBattles (NotMadeByMe)",
   Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Giangplay/Slap_Battles/main/Slap_Battles.lua"))()
   end,
})

local Button = MainTab:CreateButton({
   Name = "Fly Gui V3 (NotMadeByMe)",
   Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
   end,
})

local MiscTab = Window:CreateTab("Misc", nil) -- Title, Image
local Button = MiscTab:CreateButton({
   Name = "Teleport To Slaply (Press T PC ONLY)",
   Callback = function()
        -- Teleport Script for Slap Battles to Slaply Island

-- Correct coordinates for Slaply Island
local slaplyIslandPosition = Vector3.new(-393.1788330078125, 50.764225006103516, -13.747956275939941)  -- Slaply Island's location (Y = 1100)

-- Optional safety margin to ensure player lands on the island properly
local teleportSafetyMargin = 10  -- A buffer to prevent the player from getting stuck inside the island

-- Get the player and their character
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Function to teleport the player to Slaply Island
local function teleportToSlaplyIsland()
    -- Ensure the character exists
    if character and humanoidRootPart then
        -- Set the character's position to a safe location above Slaply Island
        humanoidRootPart.CFrame = CFrame.new(slaplyIslandPosition + Vector3.new(0, teleportSafetyMargin, 0))
    end
end

-- Connect the teleport function to the `T` key press
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessedEvent)
    -- Ignore input if it's processed by other game controls (like chat)
    if gameProcessedEvent then return end
    
    -- Check if the player pressed the "T" key
    if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.T then
        -- Teleport the player to Slaply Island
        teleportToSlaplyIsland()
    end
end)

   end,
})

local Button = MiscTab:CreateButton({
   Name = "Teleport To Moai (Press R PC ONLY)",
   Callback = function()
        -- Teleport Script for Slap Battles to Moai Location

-- Correct coordinates for Moai location
local moaiPosition = Vector3.new(213.720458984375, -15.716063499450684, -10.204557418823242)  -- Moai statue position (Y = 100, Z = -1000)

-- Optional safety margin to ensure the player lands safely
local teleportSafetyMargin = 10  -- A small buffer to ensure the player doesn't fall through or collide

-- Get the player and their character
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Function to teleport the player to Moai
local function teleportToMoai()
    -- Ensure the character exists
    if character and humanoidRootPart then
        -- Set the character's position to the Moai location
        humanoidRootPart.CFrame = CFrame.new(moaiPosition + Vector3.new(0, teleportSafetyMargin, 0))
    end
end

-- Connect the teleport function to the `R` key press
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessedEvent)
    -- Ignore input if it's processed by other game controls (like chat)
    if gameProcessedEvent then return end
    
    -- Check if the player pressed the "R" key
    if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.R then
        -- Teleport the player to Moai
        teleportToMoai()
    end
end)

   end,
})

local Button = MiscTab:CreateButton({
   Name = "Teleport To Default (Press Y PC ONLY)",
   Callback = function()
        -- Teleport Script for Slap Battles to Moai Location

-- Correct coordinates for Moai location
local moaiPosition = Vector3.new(142.1498565673828, 359.9842224121094, -3.7191057205200195)  -- Moai statue position (Y = 100, Z = -1000)

-- Optional safety margin to ensure the player lands safely
local teleportSafetyMargin = 10  -- A small buffer to ensure the player doesn't fall through or collide

-- Get the player and their character
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Function to teleport the player to Moai
local function teleportToMoai()
    -- Ensure the character exists
    if character and humanoidRootPart then
        -- Set the character's position to the Moai location
        humanoidRootPart.CFrame = CFrame.new(moaiPosition + Vector3.new(0, teleportSafetyMargin, 0))
    end
end

-- Connect the teleport function to the `Y` key press
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessedEvent)
    -- Ignore input if it's processed by other game controls (like chat)
    if gameProcessedEvent then return end
    
    -- Check if the player pressed the "Y" key
    if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.Y then
        -- Teleport the player to Moai
        teleportToMoai()
    end
end)

   end,
})

local Button = MiscTab:CreateButton({
   Name = "Teleport To Slaply (Mobile Tap On It)",
   Callback = function()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-395.93182373046875, 50.764225006103516, -16.913148880004883)
   end,
})

local Button = MiscTab:CreateButton({
   Name = "Teleport To Moai (Mobile Tap On It)",
   Callback = function()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(208.78582763671875, -15.716063499450684, -8.843342781066895)
   end,
})

local Button = MiscTab:CreateButton({
   Name = "Teleport To Default (Mobile Tap On It)",
   Callback = function()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(134.2162322998047, 359.9842224121094, -15.451227188110352)
   end,
})

local MainTab = Window:CreateTab("Badges", nil) -- Title, Image
local Button = MainTab:CreateButton({
   Name = "x2 Execution Get Untitled (Click Twice One on lobby other inside the game)",
   Callback = function()
        if game.PlaceId ~= 115782629143468 then
game:GetService("TeleportService"):Teleport(115782629143468)
else
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, 250, 0)
end
--You can execute this script two time, i mean use it in slap battle to teleport to Toh and execute it again to get the glove.
   end,
}) 

{
    TextColor = Color3.fromRGB(240, 240, 240),

    Background = Color3.fromRGB(25, 25, 25),
    Topbar = Color3.fromRGB(34, 34, 34),
    Shadow = Color3.fromRGB(20, 20, 20),

    NotificationBackground = Color3.fromRGB(100, 50, 30),
    NotificationActionsBackground = Color3.fromRGB(230, 230, 230),

    TabBackground = Color3.fromRGB(80, 80, 80),
    TabStroke = Color3.fromRGB(85, 85, 85),
    TabBackgroundSelected = Color3.fromRGB(210, 210, 210),
    TabTextColor = Color3.fromRGB(240, 240, 240),
    SelectedTabTextColor = Color3.fromRGB(50, 50, 50),

    ElementBackground = Color3.fromRGB(35, 35, 35),
    ElementBackgroundHover = Color3.fromRGB(40, 40, 40),
    SecondaryElementBackground = Color3.fromRGB(25, 25, 25),
    ElementStroke = Color3.fromRGB(50, 50, 50),
    SecondaryElementStroke = Color3.fromRGB(40, 40, 40),
            
    SliderBackground = Color3.fromRGB(50, 138, 220),
    SliderProgress = Color3.fromRGB(50, 138, 220),
    SliderStroke = Color3.fromRGB(58, 163, 255),

    ToggleBackground = Color3.fromRGB(30, 30, 30),
    ToggleEnabled = Color3.fromRGB(0, 146, 214),
    ToggleDisabled = Color3.fromRGB(100, 100, 100),
    ToggleEnabledStroke = Color3.fromRGB(0, 170, 255),
    ToggleDisabledStroke = Color3.fromRGB(125, 125, 125),
    ToggleEnabledOuterStroke = Color3.fromRGB(100, 100, 100),
    ToggleDisabledOuterStroke = Color3.fromRGB(65, 65, 65),

    DropdownSelected = Color3.fromRGB(40, 40, 40),
    DropdownUnselected = Color3.fromRGB(30, 30, 30),

    InputBackground = Color3.fromRGB(30, 30, 30),
    InputStroke = Color3.fromRGB(65, 65, 65),
    PlaceholderColor = Color3.fromRGB(178, 178, 178)
}
