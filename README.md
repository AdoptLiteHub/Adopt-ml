local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/AdoptLiteHub/Adopt-lib/refs/heads/main/README.md", true))()

local window = library:AddWindow("Muscle Legend Adopt", {
    main_color = Color3.fromRGB(64, 64, 64), -- Color
    min_size = Vector2.new(400, 346), -- Size of the gui
    can_resize = false, -- true or false
})

local Maintab = window:AddTab("Main")

Maintab:AddButton("Anti Crash",function()
	wait(0.5)local ba=Instance.new("ScreenGui") local ca=Instance.new("TextLabel")local da=Instance.new("Frame") local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,370,0,52) ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk"ca.TextColor3=Color3.new(0,1,1) ca.TextSize=22;da.Parent=ca da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0) da.Size=UDim2.new(0,370,0,107)_b.Parent=da _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0) _b.Size=UDim2.new(0,370,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by luca#5432" _b.TextColor3=Color3.new(0,1,1)_b.TextSize=20;ab.Parent=da ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377,0) ab.Size=UDim2.new(0,370,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Active" ab.TextColor3=Color3.new(0,1,1)ab.TextSize=20;local bb=game:service'VirtualUser' game:service'Players'.LocalPlayer.Idled:connect(function() bb:CaptureController()bb:ClickButton2(Vector2.new()) ab.Text="Roblox tried kicking you buy I didnt let them!"wait(2)ab.Text="Status : Active"end) 
end)

MainTab:AddButton("Destroy Ad teleport", function() -- Find and destroy the part named "RobloxForwardPortals" local part = workspace:FindFirstChild("RobloxForwardPortals") if part then part:Destroy() print("Part 'RobloxForwardPortals' has been destroyed.") else print("Part 'RobloxForwardPortals' not found.") end end)

local Killtab = window:AddTab("Kill")

local switch = Killtab:AddSwitch("Auto Kill", function(bool)
    if bool then
        -- Start teleporting heads to your right hand when the switch is turned on
        teleportHeadsToRightHand = true
        while teleportHeadsToRightHand do
            for _, player in pairs(game:GetService("Players"):GetPlayers()) do
                if player.Character and player.Character:FindFirstChild("Head") then
                    -- Check if the player is not in the whitelist
                    if not whitelist[player.Name] then
                        local head = player.Character.Head
                        local rightHand = game.Players.LocalPlayer.Character.RightHand
                        -- Teleport the player's head to your right hand
                        head.CFrame = rightHand.CFrame * CFrame.new(0, 0, 0)  -- Adjust this if necessary
                    end
                end
            end
            wait(0.1)  -- This will refresh every 0.1 seconds
        end
    else
        -- Stop teleporting when the switch is turned off
        teleportHeadsToRightHand = false
    end
end)

switch:Set(true)

local whitelist = {}

Killtab:AddTextBox("Whitelist", function(text)
    -- Add username to whitelist when the text box is used
    whitelist[text] = true
end)

Killtab:AddButton("Clear Whitelist", function()
    -- Clear the whitelist when the button is clicked
    whitelist = {}
end)


Kill:AddLabel("")

local targetPlayerName = ""  -- Variable to store the target player's name
local teleporting = false  -- Flag to control teleportation

-- Textbox to write player name
Kill:AddTextBox("Write Player Name to kill", function(text)
    targetPlayerName = text  -- Store the player name when typed in the textbox
end)

local switch = Kill:AddSwitch("Kill Player", function(bool)
    if bool then
        -- Start teleporting the target player's head to your right hand when the toggle is turned on
        teleporting = true
        while teleporting do
            if targetPlayerName ~= "" then
                local targetPlayer = game:GetService("Players"):FindFirstChild(targetPlayerName)
                if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("Head") then
                    local head = targetPlayer.Character.Head
                    local rightHand = game.Players.LocalPlayer.Character.RightHand
                    -- Teleport the player's head to your right hand
                    head.CFrame = rightHand.CFrame * CFrame.new(0, 0, 0)  -- Adjust this if necessary
                end
            end
            wait(0.1)  -- Update every 0.1 seconds
        end
    else
        -- Stop teleporting when the toggle is turned off
        teleporting = false
    end
end)

switch:Set(true)

-- Button to clear the player name written in the textbox
Kill:AddButton("Clear Player", function()
    targetPlayerName = ""  -- Clear the target player name
end)


KillTab:AddButton("Speed Punch", function() local player = game.Players.LocalPlayer local punch = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch") if punch and punch:FindFirstChild("attackTime") then punch.attackTime.Value = 0 end end) local AutoPunchToggle = false KillTab:AddSwitch("Auto Punch", function(State) AutoPunchToggle = State if AutoPunchToggle then local function autoPunchLoop() while AutoPunchToggle do local player = game.Players.LocalPlayer local tool = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch") if tool and tool.Parent ~= player.Character then tool.Parent = player.Character end if tool then tool:Activate() end wait(0.01) end end coroutine.wrap(autoPunchLoop)() end end)

local rebirthAmount = 0 -- Store the desired rebirth amount local currentRebirths = 0 -- Track the number of rebirths performed local autoRebirthEnabled = false -- State of the Auto Rebirth toggle -- Function to handle rebirthing local function autoRebirth() spawn(function() while autoRebirthEnabled do -- Check if the rebirthRemote exists local rebirthevent = game.ReplicatedStorage.rEvents:FindFirstChild("rebirthRemote") if rebirthevent then -- Fire the rebirthRemote if the target hasn't been reached if rebirthAmount == 0 or currentRebirths < rebirthAmount then rebirthevent:FireServer() currentRebirths = currentRebirths + 1 else -- Stop rebirthing if the target is reached autoRebirthEnabled = false print("Reached the desired rebirth amount.") end end wait(0.1) -- Adjust delay as needed end end) end -- Add a Rebirth Tab local RebirthTab = Window:AddTab("Rebirth") -- Add the Auto Rebirth Toggle RebirthTab:AddSwitch("Auto Rebirth", { Default = false, Callback = function(state) autoRebirthEnabled = state if state then autoRebirth() -- Start rebirthing when enabled end end }) -- Add a Textbox for Rebirth Amount RebirthTab:AddTextBox("Select Rebirth Amount", function(text) local inputNumber = tonumber(text) -- Convert input text to a number if inputNumber and inputNumber >= 0 then rebirthAmount = inputNumber -- Set the target rebirth amount currentRebirths = 0 -- Reset the counter print("Rebirth amount set to:", rebirthAmount) else print("Invalid input. Please enter a positive number.") end end)

local AutoFarmTab = Window:AddTab("Auto Farm") local AutoWeightToggle = false local AutoPushupsToggle = false local SitupsToggle = false local MuscleKingFarmToggle = false -- Auto Punch local function AutoPunch() spawn(function() while AutoPunchToggle do local player = game.Players.LocalPlayer local punchTool = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch") if punchTool then player.Character.Humanoid:EquipTool(punchTool) punchTool:Activate() end wait(0.1) end end) end -- Auto Weight local function AutoWeight() spawn(function() while AutoWeightToggle do local player = game.Players.LocalPlayer local weightTool = player.Backpack:FindFirstChild("Weight") or player.Character:FindFirstChild("Weight") if weightTool then player.Character.Humanoid:EquipTool(weightTool) weightTool:Activate() end wait(0.1) end end) end -- Auto Pushups local function AutoPushups() spawn(function() while AutoPushupsToggle do local player = game.Players.LocalPlayer local pushupsTool = player.Backpack:FindFirstChild("Pushups") or player.Character:FindFirstChild("Pushups") if pushupsTool then player.Character.Humanoid:EquipTool(pushupsTool) pushupsTool:Activate() end wait(0.1) end end) end -- Auto Situps local function AutoSitups() spawn(function() while SitupsToggle do local player = game.Players.LocalPlayer local situpsTool = player.Backpack:FindFirstChild("Situps") or player.Character:FindFirstChild("Situps") if situpsTool then player.Character.Humanoid:EquipTool(situpsTool) situpsTool:Activate() end wait(0.1) end end) end -- Muscle King Farm local function MuscleKingFarm() spawn(function() while MuscleKingFarmToggle do local player = game.Players.LocalPlayer player.Character.HumanoidRootPart.CFrame = CFrame.new(-8546.25879, 23.045435, -5636.78418) local pushupsTool = player.Backpack:FindFirstChild("Pushups") or player.Character:FindFirstChild("Pushups") if pushupsTool then player.Character.Humanoid:EquipTool(pushupsTool) pushupsTool:Activate() end wait(0.1) end end) end AutoFarmTab:AddSwitch("Auto Weight", function(value) AutoWeightToggle = value if AutoWeightToggle then AutoWeight() end end) AutoFarmTab:AddSwitch("Auto Pushups", function(value) AutoPushupsToggle = value if AutoPushupsToggle then AutoPushups() end end) AutoFarmTab:AddSwitch("Situps", function(value) SitupsToggle = value if SitupsToggle then AutoSitups() end end) AutoFarmTab:AddSwitch("Muscle King Farm", function(value) MuscleKingFarmToggle = value if MuscleKingFarmToggle then MuscleKingFarm() end end) -- Rock Tab local RockTab = Window:AddTab("Rock") -- Tiny Rock Teleport RockTab:AddButton("Tiny Rock", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(17.6410236, -1.30998898, 2106.48926) end) -- Frozen Rock Teleport RockTab:AddButton("Frozen Rock", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2551.75854, -0.359962642, -243.308777) end) -- Mystic Rock Teleport RockTab:AddButton("Mystic Rock", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2186.14111, -0.359961987, 1250.59802) end) -- Inferno Rock Teleport RockTab:AddButton("Inferno Rock", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-7262.18701, -0.359961987, -1259.24426) end) -- Legend Rock Teleport RockTab:AddButton("Legend Rock", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(4140.41797, 987.453186, -4089.34937) end) -- MuscleKing Rock Teleport RockTab:AddButton("Muscle King Rock", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-8971.56641, 27.4031715, -6061.27734) end) -- Teleport Tab local TeleportTab = Window:AddTab("Teleport") -- Beach Teleport TeleportTab:AddButton("Beach", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-11, 5, -178) end) -- Legends Teleport TeleportTab:AddButton("Legends", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(4603, 989, -3898) end) -- Muscle Teleport TeleportTab:AddButton("Muscle", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-8626, 15, -5730) end) -- Tiny Teleport TeleportTab:AddButton("Tiny", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-38, 5, 1884) end) -- Secret Teleport TeleportTab:AddButton("Secret", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2596, -1, 5738) end) -- Inferno Teleport TeleportTab:AddButton("Inferno", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-6759, 5, -1285) end) -- Frost Teleport TeleportTab:AddButton("Frost", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2623, 5, -409) end) -- Mythical Teleport TeleportTab:AddButton("Mythical", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2251, 5, 1073) end)
