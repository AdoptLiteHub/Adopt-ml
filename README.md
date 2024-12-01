local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/memejames/elerium-v2-ui-library/main/Library", true))()

local window = library:AddWindow("Muscle Legend Adopt", {
    main_color = Color3.fromRGB(64, 64, 64), -- Color
    min_size = Vector2.new(400, 346), -- Size of the gui
    can_resize = false, -- true or false
})

local Maintab = window:AddTab("Main")

-- Anti-Afk Button
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

    local bb = game:GetService('VirtualUser')
    game:GetService('Players').LocalPlayer.Idled:connect(function()
        bb:CaptureController()
        bb:ClickButton2(Vector2.new())
        ab.Text = "Roblox tried kicking you but I didn't let them!"
        wait(2)
        ab.Text = "Status: Active"
    end)
end)

-- Destroy "RobloxForwardPortals" part
Maintab:AddButton("Destroy Ad teleport", function()
    -- Find and destroy the part named "RobloxForwardPortals"
    local part = workspace:FindFirstChild("RobloxForwardPortals")
    if part then
        part:Destroy()
        print("Part 'RobloxForwardPortals' has been destroyed.")
    else
        print("Part 'RobloxForwardPortals' not found.")
    end
end)

local Killtab = window:AddTab("Kill")
local whitelist = {}

-- Auto Kill Switch
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

-- Whitelist Textbox
Killtab:AddTextBox("Whitelist", function(text)
    -- Add username to whitelist when the text box is used
    whitelist[text] = true
end)

-- Clear Whitelist Button
Killtab:AddButton("Clear Whitelist", function()
    -- Clear the whitelist when the button is clicked
    whitelist = {}
end)

local targetPlayerName = ""  -- Variable to store the target player's name
local teleporting = false  -- Flag to control teleportation

-- Textbox to write player name
Killtab:AddTextBox("Write Player Name to kill", function(text)
    targetPlayerName = text  -- Store the player name when typed in the textbox
end)

-- Kill Player Switch
local switchKill = Killtab:AddSwitch("Kill Player", function(bool)
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

switchKill:Set(true)

-- Button to clear the player name written in the textbox
Killtab:AddButton("Clear Player", function()
    targetPlayerName = ""  -- Clear the target player name
end)

-- Auto Punch Switch
local autoPunchActive = false
local punchSwitch = Killtab:AddSwitch("Auto Punch", function(bool)
    autoPunchActive = bool
    if autoPunchActive then
        -- Try to equip the Punch tool and use it infinitely
        local punchTool = game.Players.LocalPlayer.Backpack:FindFirstChild("Punch")
        -- If the tool doesn't exist in the backpack, we can equip it from somewhere else (e.g., replicated storage or starter pack)
        if not punchTool then
            local replicatedStorage = game:GetService("ReplicatedStorage")
            punchTool = replicatedStorage:WaitForChild("Punch"):Clone()
            punchTool.Parent = game.Players.LocalPlayer.Backpack -- Equip the cloned tool
        end

        -- Use the Punch tool infinitely while autoPunchActive is true
        while autoPunchActive do
            if punchTool and punchTool:IsA("Tool") and punchTool.Parent == game.Players.LocalPlayer.Backpack then
                -- Simulate the tool's activation (assuming it has an "Activated" event)
                punchTool:Activate() -- This triggers the tool's activation action
            end
            wait(0.1) -- Use the tool every 0.1 seconds
        end
    else
        -- Stop using the Punch tool when the toggle is off
        autoPunchActive = false
    end
end)

punchSwitch:Set(false)
