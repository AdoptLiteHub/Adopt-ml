-- Assuming that the library is a proper Lua module from GitHub, loading it correctly
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/AdoptLiteHub/Adopt-lib/refs/heads/main/README.md", true))()

local window = library:AddWindow("Muscle Legend Adopt", {
    main_color = Color3.fromRGB(64,64,64), -- Color
    min_size = Vector2.new(360, 400), -- Size of the gui
    can_resize = false, -- true or false
})

local Maintab = window:AddTab("Main")

-- Anti Crash Button
Maintab:AddButton("Anti Crash", function()
    wait(0.5)
    local ba = Instance.new("ScreenGui")
    local ca = Instance.new("TextLabel")
    local da = Instance.new("Frame")
    local _b = Instance.new("TextLabel")
    local ab = Instance.new("TextLabel")

    ba.Parent = game.CoreGui
    ba.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    ca.Parent = ba
    ca.Active = true
    ca.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    ca.Draggable = true
    ca.Position = UDim2.new(0.698610067, 0, 0.098096624, 0)
    ca.Size = UDim2.new(0, 370, 0, 52)
    ca.Font = Enum.Font.SourceSansSemibold
    ca.Text = "Anti Afk"
    ca.TextColor3 = Color3.new(0, 1, 1)
    ca.TextSize = 22

    da.Parent = ca
    da.BackgroundColor3 = Color3.new(0.196078, 0.196078, 0.196078)
    da.Position = UDim2.new(0, 0, 1.0192306, 0)
    da.Size = UDim2.new(0, 370, 0, 107)

    _b.Parent = da
    _b.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    _b.Position = UDim2.new(0, 0, 0.800455689, 0)
    _b.Size = UDim2.new(0, 370, 0, 21)
    _b.Font = Enum.Font.Arial
    _b.Text = "Made by luca#5432"
    _b.TextColor3 = Color3.new(0, 1, 1)
    _b.TextSize = 20

    ab.Parent = da
    ab.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    ab.Position = UDim2.new(0, 0, 0.158377, 0)
    ab.Size = UDim2.new(0, 370, 0, 44)
    ab.Font = Enum.Font.ArialBold
    ab.Text = "Status: Active"
    ab.TextColor3 = Color3.new(0, 1, 1)
    ab.TextSize = 20

    local bb = game:GetService("VirtualUser")
    game:GetService("Players").LocalPlayer.Idled:Connect(function()
        bb:CaptureController()
        bb:ClickButton2(Vector2.new())
        ab.Text = "Roblox tried kicking you, but I didnâ€™t let them!"
        wait(2)
        ab.Text = "Status : Active"
    end)
end)

-- Destroy Ad teleport Button
Maintab:AddButton("Destroy Ad teleport", function()
    local part = workspace:FindFirstChild("RobloxForwardPortals")
    if part then
        part:Destroy()
        print("Part 'RobloxForwardPortals' has been destroyed.")
    else
        print("Part 'RobloxForwardPortals' not found.")
    end
end)





-- Create folder "Islands Teleports"
local folder3 = Maintab:AddFolder("Islands Teleports")

-- Beach Teleport
folder3:AddButton("Beach", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-11, 5, -178)
end)

-- Legends Teleport
folder3:AddButton("Legends", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(4603, 989, -3898)
end)

-- Muscle Teleport
folder3:AddButton("Muscle", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-8626, 15, -5730)
end)

-- Tiny Teleport
folder3:AddButton("Tiny", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-38, 5, 1884)
end)

-- Secret Teleport
folder3:AddButton("Secret", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2596, -1, 5738)
end)

-- Inferno Teleport
folder3:AddButton("Inferno", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-6759, 5, -1285)
end)

-- Frost Teleport
folder3:AddButton("Frost", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2623, 5, -409)
end)

-- Mythical Teleport
folder3:AddButton("Mythical", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2251, 5, 1073)
end)


local Killtab = window:AddTab("Kill")
local whitelist = {}
local AutoPunchToggle = false
local AutoKillToggle = false
local targetPlayerName = ""
local teleporting = false
local playerToSpyOn = nil
local godModeToggle = false
local autoEquipPunchToggle = false

-- Speed Punch (Optional Speed Increase for Punch)
Killtab:AddButton("Speed Punch", function()
    local player = game.Players.LocalPlayer
    local punch = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch")
    if punch and punch:FindFirstChild("attackTime") then
        punch.attackTime.Value = 0
    end
end)

-- Auto Equip Punch Toggle
Killtab:AddSwitch("Auto Equip Punch", function(State)
    autoEquipPunchToggle = State
    if autoEquipPunchToggle then
        spawn(function()
            while autoEquipPunchToggle do
                local player = game.Players.LocalPlayer
                -- Equip the Punch tool if it's not already equipped
                local tool = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch")
                if not tool then
                    -- Equip the tool to the character if not already equipped
                    local punchTool = player.Backpack:FindFirstChild("Punch")
                    if punchTool then
                        punchTool.Parent = player.Character
                    end
                end
                wait(0.1)  -- Repeat the check every 0.1 seconds
            end
        end)
    end
end)

-- Auto Punch Toggle
Killtab:AddSwitch("Auto Punch", function(State)
    AutoPunchToggle = State
    if AutoPunchToggle then
        spawn(function()
            while AutoPunchToggle do
                local player = game.Players.LocalPlayer
                local tool = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch")
                if tool and tool.Parent ~= player.Character then
                    tool.Parent = player.Character
                end
                if tool then
                    tool:Activate()
                end
                wait(0.01)  -- Trigger every 0.01 seconds
            end
        end)
    end
end)

-- Auto Kill Toggle (Kill Aura)
Killtab:AddSwitch("Auto Kill", function(State)
    AutoKillToggle = State
    if AutoKillToggle then
        -- Define the global variable to control auto-kill activity
        _G.autoKillActive = true

        -- Function for auto-kill method 1
        local function method1()
            while _G.autoKillActive do
                wait()
                local player = game.Players.LocalPlayer
                if player.muscleEvent then
                    player.muscleEvent:FireServer("punch", "rightHand")
                    player.muscleEvent:FireServer("punch", "leftHand")

                    for _, otherPlayer in pairs(game.Players:GetChildren()) do
                        if otherPlayer.Name ~= player.Name then
                            local character = game.Workspace:FindFirstChild(otherPlayer.Name)
                            local localCharacter = game.Workspace:FindFirstChild(player.Name)

                            if character and localCharacter then
                                local leftHand = localCharacter:FindFirstChild("LeftHand")
                                if leftHand then
                                    local head = character:FindFirstChild("Head")
                                    if head then
                                        head.CFrame = leftHand.CFrame
                                    end

                                    for _, descendant in pairs(character:GetDescendants()) do
                                        if descendant:IsA("BasePart") and descendant.Name == "Handle" then
                                            descendant.CFrame = leftHand.CFrame
                                        end
                                    end

                                    local sweatPart = character:FindFirstChild("sweatPart")
                                    if sweatPart then
                                        sweatPart.CFrame = leftHand.CFrame
                                    end
                                end
                            end
                        end
                    end
                end
            end
        end

        -- Function for auto-kill method 2
        local function method2()
            while _G.autoKillActive do
                wait()
                local player = game.Players.LocalPlayer
                if player.muscleEvent then
                    player.muscleEvent:FireServer("punch", "rightHand")
                    player.muscleEvent:FireServer("punch", "leftHand")

                    for _, otherPlayer in pairs(game.Players:GetChildren()) do
                        if otherPlayer.Name ~= player.Name then
                            local character = game.Workspace:FindFirstChild(otherPlayer.Name)
                            local localCharacter = game.Workspace:FindFirstChild(player.Name)

                            if character and localCharacter then
                                local leftHand = localCharacter:FindFirstChild("LeftHand")
                                if leftHand then
                                    local head = character:FindFirstChild("Head")
                                    if head then
                                        head.Parent = nil
                                        wait(0.1)
                                        head.CFrame = leftHand.CFrame
                                        head.Parent = character
                                    end

                                    for _, descendant in pairs(character:GetDescendants()) do
                                        if descendant:IsA("BasePart") and descendant.Name == "Handle" then
                                            descendant.CFrame = leftHand.CFrame
                                        end
                                    end

                                    local sweatPart = character:FindFirstChild("sweatPart")
                                    if sweatPart then
                                        sweatPart.CFrame = leftHand.CFrame
                                    end
                                end
                            end
                        end
                    end
                end
            end
        end

        -- Run both methods concurrently
        coroutine.wrap(method1)()
        coroutine.wrap(method2)()
    else
        _G.autoKillActive = false
    end
end)

-- Whitelist toggle (Add player to whitelist via chat)
Killtab:AddTextBox("Whitelist (Player Name)", function(text)
    if text ~= "" then
        whitelist[text] = true -- Add the player to the whitelist
        print(text .. " has been added to the whitelist.")
    else
        print("Invalid input.")
    end
end)

-- Label for Kill Target
Killtab:AddLabel("Target Player for Kill")

-- Textbox to input the target player names (for Kill Target)
Killtab:AddTextBox("Target Player", function(text)
    targetPlayerName = text -- Set the target player name when you input text
end)

-- Target Kill (This is an extra kill function for targeting a specific player)
Killtab:AddSwitch("Kill Target", function(State)
    if State then
        spawn(function()
            while true do
                wait(0.1)  -- You can adjust the rate of this action
                local targetPlayer = game.Players:FindFirstChild(targetPlayerName)
                if targetPlayer then
                    local targetCharacter = targetPlayer.Character
                    if targetCharacter and targetCharacter:FindFirstChild("Head") then
                        -- Apply a punch to the target
                        local muscleEvent = game.Players.LocalPlayer:WaitForChild("muscleEvent")
                        local argsRight = { [1] = "punch", [2] = "rightHand" }
                        local argsLeft = { [1] = "punch", [2] = "leftHand" }
                        muscleEvent:FireServer(unpack(argsRight))  -- Right Hand Punch
                        muscleEvent:FireServer(unpack(argsLeft))   -- Left Hand Punch
                    end
                end
            end
        end)
    end
end)

-- Spy Toggle
Killtab:AddSwitch("Spy", function(State)
    if State then
        local player = game.Players.LocalPlayer
        spawn(function()
            while true do
                wait(0.1)  -- Check every 0.1 seconds
                if playerToSpyOn then
                    local targetPlayer = game.Players:FindFirstChild(playerToSpyOn)
                    if targetPlayer and targetPlayer.Character then
                        -- Teleport the LocalPlayer to the target player's head (or wherever you want)
                        local targetHead = targetPlayer.Character:FindFirstChild("Head")
                        if targetHead then
                            player.Character:MoveTo(targetHead.Position)
                        end
                    end
                end
            end
        end)
    end
end)

local Killtab = window:AddTab("Kill")
local whitelist = {}
local AutoPunchToggle = false
local AutoKillToggle = false
local targetPlayerName = ""
local teleporting = false
local playerToSpyOn = nil
local godModeToggle = false
local autoEquipPunchToggle = false

-- Speed Punch (Optional Speed Increase for Punch)
Killtab:AddButton("Speed Punch", function()
    local player = game.Players.LocalPlayer
    local punch = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch")
    if punch and punch:FindFirstChild("attackTime") then
        punch.attackTime.Value = 0
    end
end)

-- Auto Equip Punch Toggle
Killtab:AddSwitch("Auto Equip Punch", function(State)
    autoEquipPunchToggle = State
    if autoEquipPunchToggle then
        spawn(function()
            while autoEquipPunchToggle do
                local player = game.Players.LocalPlayer
                -- Equip the Punch tool if it's not already equipped
                local tool = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch")
                if not tool then
                    -- Equip the tool to the character if not already equipped
                    local punchTool = player.Backpack:FindFirstChild("Punch")
                    if punchTool then
                        punchTool.Parent = player.Character
                    end
                end
                wait(0.1)  -- Repeat the check every 0.1 seconds
            end
        end)
    end
end)

-- Auto Punch Toggle
Killtab:AddSwitch("Auto Punch", function(State)
    AutoPunchToggle = State
    if AutoPunchToggle then
        spawn(function()
            while AutoPunchToggle do
                local player = game.Players.LocalPlayer
                local tool = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch")
                if tool and tool.Parent ~= player.Character then
                    tool.Parent = player.Character
                end
                if tool then
                    tool:Activate()
                end
                wait(0.01)  -- Trigger every 0.01 seconds
            end
        end)
    end
end)

-- Auto Kill Toggle (Kill Aura)
Killtab:AddSwitch("Auto Kill", function(State)
    AutoKillToggle = State
    if AutoKillToggle then
        -- Define the global variable to control auto-kill activity
        _G.autoKillActive = true

        -- Function for auto-kill method 1
        local function method1()
            while _G.autoKillActive do
                wait()
                local player = game.Players.LocalPlayer
                if player.muscleEvent then
                    player.muscleEvent:FireServer("punch", "rightHand")
                    player.muscleEvent:FireServer("punch", "leftHand")

                    for _, otherPlayer in pairs(game.Players:GetChildren()) do
                        if otherPlayer.Name ~= player.Name then
                            local character = game.Workspace:FindFirstChild(otherPlayer.Name)
                            local localCharacter = game.Workspace:FindFirstChild(player.Name)

                            if character and localCharacter then
                                local leftHand = localCharacter:FindFirstChild("LeftHand")
                                if leftHand then
                                    local head = character:FindFirstChild("Head")
                                    if head then
                                        head.CFrame = leftHand.CFrame
                                    end

                                    for _, descendant in pairs(character:GetDescendants()) do
                                        if descendant:IsA("BasePart") and descendant.Name == "Handle" then
                                            descendant.CFrame = leftHand.CFrame
                                        end
                                    end

                                    local sweatPart = character:FindFirstChild("sweatPart")
                                    if sweatPart then
                                        sweatPart.CFrame = leftHand.CFrame
                                    end
                                end
                            end
                        end
                    end
                end
            end
        end

        -- Function for auto-kill method 2
        local function method2()
            while _G.autoKillActive do
                wait()
                local player = game.Players.LocalPlayer
                if player.muscleEvent then
                    player.muscleEvent:FireServer("punch", "rightHand")
                    player.muscleEvent:FireServer("punch", "leftHand")

                    for _, otherPlayer in pairs(game.Players:GetChildren()) do
                        if otherPlayer.Name ~= player.Name then
                            local character = game.Workspace:FindFirstChild(otherPlayer.Name)
                            local localCharacter = game.Workspace:FindFirstChild(player.Name)

                            if character and localCharacter then
                                local leftHand = localCharacter:FindFirstChild("LeftHand")
                                if leftHand then
                                    local head = character:FindFirstChild("Head")
                                    if head then
                                        head.Parent = nil
                                        wait(0.1)
                                        head.CFrame = leftHand.CFrame
                                        head.Parent = character
                                    end

                                    for _, descendant in pairs(character:GetDescendants()) do
                                        if descendant:IsA("BasePart") and descendant.Name == "Handle" then
                                            descendant.CFrame = leftHand.CFrame
                                        end
                                    end

                                    local sweatPart = character:FindFirstChild("sweatPart")
                                    if sweatPart then
                                        sweatPart.CFrame = leftHand.CFrame
                                    end
                                end
                            end
                        end
                    end
                end
            end
        end

        -- Run both methods concurrently
        coroutine.wrap(method1)()
        coroutine.wrap(method2)()
    else
        _G.autoKillActive = false
    end
end)

-- Whitelist toggle (Add player to whitelist via chat)
Killtab:AddTextBox("Whitelist (Player Name)", function(text)
    if text ~= "" then
        whitelist[text] = true -- Add the player to the whitelist
        print(text .. " has been added to the whitelist.")
    else
        print("Invalid input.")
    end
end)

-- Label for Kill Target
Killtab:AddLabel("Target Player for Kill")

-- Textbox to input the target player names (for Kill Target)
Killtab:AddTextBox("Target Player", function(text)
    targetPlayerName = text -- Set the target player name when you input text
end)

-- Kill Target Toggle (Kill only the specified target player)
Killtab:AddSwitch("Kill Target", function(State)
    if State then
        spawn(function()
            while true do
                wait(0.1)  -- You can adjust the rate of this action
                local targetPlayer = game.Players:FindFirstChild(targetPlayerName)
                if targetPlayer then
                    local targetCharacter = targetPlayer.Character
                    if targetCharacter and targetCharacter:FindFirstChild("Head") then
                        -- Apply a punch to the target
                        local muscleEvent = game.Players.LocalPlayer:WaitForChild("muscleEvent")
                        local argsRight = { [1] = "punch", [2] = "rightHand" }
                        local argsLeft = { [1] = "punch", [2] = "leftHand" }
                        muscleEvent:FireServer(unpack(argsRight))  -- Right Hand Punch
                        muscleEvent:FireServer(unpack(argsLeft))   -- Left Hand Punch
                    end
                end
            end
        end)
    end
end)

-- Spy Toggle
Killtab:AddSwitch("Spy", function(State)
    if State then
        local player = game.Players.LocalPlayer
        spawn(function()
            while true do
                wait(0.1)  -- Check every 0.1 seconds
                if playerToSpyOn then
                    local targetPlayer = game.Players:FindFirstChild(playerToSpyOn)
                    if targetPlayer and targetPlayer.Character then
                        -- Teleport the LocalPlayer to the target player's head (or wherever you want)
                        local targetHead = targetPlayer.Character:FindFirstChild("Head")
                        if targetHead then
                            player.Character:MoveTo(targetHead.Position)
                        end
                    end
                end
            end
        end)
    end
end)

-- Textbox for Spy target player
Killtab:AddTextBox("Spy on Player (Name)", function(text)
    playerToSpyOn = text
end)

-- God Mode Toggle
Killtab:AddSwitch("God Mode", function(State)
    godModeToggle = State
    if godModeToggle then
        -- Repeat the joinBrawl event every 0.1 seconds to simulate God Mode
        spawn(function()
            while godModeToggle do
                local args = {
                    [1] = "joinBrawl"
                }
                game:GetService("ReplicatedStorage"):WaitForChild("rEvents"):WaitForChild("brawlEvent"):FireServer(unpack(args))
                wait()  -- Repeat the event every 0.1 seconds
            end
        end)
    end
end)








local rebirthAmount = 0
local currentRebirths = 0
local autoRebirthEnabled = false

local function autoRebirth()
    spawn(function()
        while autoRebirthEnabled do
            local rebirthevent = game.ReplicatedStorage.rEvents:FindFirstChild("rebirthRemote")
            if rebirthevent then
                if rebirthAmount == 0 or currentRebirths < rebirthAmount then
                    rebirthevent:FireServer()
                    currentRebirths = currentRebirths + 1
                else
                    autoRebirthEnabled = false
                    print("Reached the desired rebirth amount.")
                end
            end
            wait(0.1)
        end
    end)
end

local RebirthTab = window:AddTab("Rebirth")

RebirthTab:AddSwitch("Auto Rebirth", {
    Default = false,
    Callback = function(state)
        autoRebirthEnabled = state
        if state then
            autoRebirth()
        end
    end
})

RebirthTab:AddTextBox("Select Rebirth Amount", function(text)
    local inputNumber = tonumber(text)
    if inputNumber and inputNumber >= 0 then
        rebirthAmount = inputNumber
        currentRebirths = 0
        print("Rebirth amount set to:", rebirthAmount)
    else
        print("Invalid input. Please enter a positive number.")
    end
end)

local AutoFarmTab = window:AddTab("Auto Farm")
local AutoWeightToggle = false
local AutoPushupsToggle = false
local SitupsToggle = false
local MuscleKingFarmToggle = false

-- Helper functions for auto actions
local function AutoWeight()
    while AutoWeightToggle do
        local player = game.Players.LocalPlayer
        local weightTool = player.Backpack:FindFirstChild("Weight") or player.Character:FindFirstChild("Weight")
        if weightTool then
            player.Character.Humanoid:EquipTool(weightTool)
            weightTool:Activate()
        end
        wait(0.1)
    end
end

local function AutoPushups()
    while AutoPushupsToggle do
        local player = game.Players.LocalPlayer
        local pushupTool = player.Backpack:FindFirstChild("Pushup") or player.Character:FindFirstChild("Pushup")
        if pushupTool then
            player.Character.Humanoid:EquipTool(pushupTool)
            pushupTool:Activate()
        end
        wait(0.1)
    end
end

local function AutoSitups()
    while SitupsToggle do
        local player = game.Players.LocalPlayer
        local situpTool = player.Backpack:FindFirstChild("Situp") or player.Character:FindFirstChild("Situp")
        if situpTool then
            player.Character.Humanoid:EquipTool(situpTool)
            situpTool:Activate()
        end
        wait(0.1)
    end
end

local function MuscleKingFarm()
    while MuscleKingFarmToggle do
        local player = game.Players.LocalPlayer
        local muscleKingTool = player.Backpack:FindFirstChild("MuscleKing") or player.Character:FindFirstChild("MuscleKing")
        if muscleKingTool then
            player.Character.Humanoid:EquipTool(muscleKingTool)
            muscleKingTool:Activate()
        end
        wait(0.1)
    end
end

AutoFarmTab:AddSwitch("Auto Weight", function(State)
    AutoWeightToggle = State
    if AutoWeightToggle then
        AutoWeight()
    end
end)

AutoFarmTab:AddSwitch("Auto Pushups", function(value)
    AutoPushupsToggle = value
    if AutoPushupsToggle then
        AutoPushups()
    end
end)

AutoFarmTab:AddSwitch("Situps", function(value)
    SitupsToggle = value
    if SitupsToggle then
        AutoSitups()
    end
end)

AutoFarmTab:AddSwitch("Muscle King Farm", function(value)
    MuscleKingFarmToggle = value
    if MuscleKingFarmToggle then
        MuscleKingFarm()
    end
end)



-- View Stats Tab
local ViewStatsTab = window:AddTab("ViewStats")

local playerData = {}
local currentSelectedPlayer = nil
local notFoundLabel = nil
local selectedPlayerName = nil

-- Helper function to abbreviate large numbers
local function abbreviateNumber(value)
    if value >= 1e15 then
        return string.format("%.1fQa", value / 1e15)
    elseif value >= 1e12 then
        return string.format("%.1fT", value / 1e12)
    elseif value >= 1e9 then
        return string.format("%.1fB", value / 1e9)
    elseif value >= 1e6 then
        return string.format("%.1fM", value / 1e6)
    elseif value >= 1e3 then
        return string.format("%.1fK", value / 1e3)
    else
        return tostring(value)
    end
end

-- Function to create labels for the selected player's stats
local function createPlayerLabels(player)
    local playerName = player.Name
    local leaderstats = player:FindFirstChild("leaderstats")
    local equippedPets = player:FindFirstChild("equippedPets")
    local ownedGamepasses = player:FindFirstChild("ownedGamepasses")

    -- Create labels for stats
    local labels = {
        StrengthLabel = ViewStatsTab:AddLabel("Strength: " .. abbreviateNumber(leaderstats.Strength.Value or 0)),
        DurabilityLabel = ViewStatsTab:AddLabel("Durability: " .. abbreviateNumber(player.Durability.Value or 0)),
        KillsLabel = ViewStatsTab:AddLabel("Kills: " .. abbreviateNumber(leaderstats.Kills.Value or 0)),
        BrawlsLabel = ViewStatsTab:AddLabel("Brawls: " .. abbreviateNumber(leaderstats.Brawls.Value or 0)),
        AgilityLabel = ViewStatsTab:AddLabel("Agility: " .. abbreviateNumber(player.Agility.Value or 0)),
        EvilKarmaLabel = ViewStatsTab:AddLabel("evilKarma: " .. abbreviateNumber(player.evilKarma.Value or 0)),
        GoodKarmaLabel = ViewStatsTab:AddLabel("goodKarma: " .. abbreviateNumber(player.goodKarma.Value or 0)),
        MapLabel = ViewStatsTab:AddLabel("Map: " .. (player.currentMap.Value or "N/A")),
        KingTimeLabel = ViewStatsTab:AddLabel("KingTime: " .. abbreviateNumber(player.muscleKingTime.Value or 0)),
        PremiumLabel = ViewStatsTab:AddLabel("Premium: " .. (player.MembershipType == Enum.MembershipType.Premium and "true" or "false")),
    }

    -- Add pet labels
    for i = 1, 5 do
        local petValue = equippedPets:FindFirstChild("pet" .. i) and equippedPets["pet" .. i].Value or "N/A"
        labels["Pet" .. i .. "Label"] = ViewStatsTab:AddLabel("Pet" .. i .. ": " .. tostring(petValue))
    end

    -- Add owned gamepasses
    local gamepassList = {}
    if ownedGamepasses then
        for _, gamepass in ipairs(ownedGamepasses:GetChildren()) do
            table.insert(gamepassList, gamepass.Name)
        end
    end

    local gamepassesText = #gamepassList > 0 and table.concat(gamepassList, ", ") or "N/A"
    labels.GamepassesLabel = ViewStatsTab:AddLabel("ownedGamepasses: " .. gamepassesText)

    playerData[playerName] = labels

    -- Connect value change events to update the labels
    leaderstats.Kills.Changed:Connect(function()
        labels.KillsLabel.Text = "Kills: " .. abbreviateNumber(leaderstats.Kills.Value or 0)
    end)

    leaderstats.Strength.Changed:Connect(function()
        labels.StrengthLabel.Text = "Strength: " .. abbreviateNumber(leaderstats.Strength.Value or 0)
    end)

    leaderstats.Brawls.Changed:Connect(function()
        labels.BrawlsLabel.Text = "Brawls: " .. abbreviateNumber(leaderstats.Brawls.Value or 0)
    end)

    player.Durability.Changed:Connect(function()
        labels.DurabilityLabel.Text = "Durability: " .. abbreviateNumber(player.Durability.Value or 0)
    end)

    player.Agility.Changed:Connect(function()
        labels.AgilityLabel.Text = "Agility: " .. abbreviateNumber(player.Agility.Value or 0)
    end)

    player.evilKarma.Changed:Connect(function()
        labels.EvilKarmaLabel.Text = "evilKarma: " .. abbreviateNumber(player.evilKarma.Value or 0)
    end)

    player.goodKarma.Changed:Connect(function()
        labels.GoodKarmaLabel.Text = "goodKarma: " .. abbreviateNumber(player.goodKarma.Value or 0)
    end)
end

-- Function to remove player labels (cleanup)
local function removePlayerLabels(playerName)
    if playerData[playerName] then
        for _, label in pairs(playerData[playerName]) do
            label:Remove()
        end
        playerData[playerName] = nil
    end
end

-- Adding a textbox for player name input
local textbox = ViewStatsTab:AddTextBox("Player Name", function(playerName)
    selectedPlayerName = playerName
    if notFoundLabel then
        notFoundLabel:Remove()
        notFoundLabel = nil
    end

    local player = game.Players:FindFirstChild(playerName)
    if player then
        if currentSelectedPlayer then
            removePlayerLabels(currentSelectedPlayer)
        end
        createPlayerLabels(player)
        currentSelectedPlayer = playerName
    else
        notFoundLabel = ViewStatsTab:AddLabel("Player not found!")
    end
end)




-- Show the window
window:Show()
