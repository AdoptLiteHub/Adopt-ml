local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/AdoptLiteHub/Adopt-lib/refs/heads/main/README.md", true))()

local window = library:AddWindow("Muscle Legend Adopt", {
    main_color = Color3.fromRGB(64, 64, 64), -- Color
    min_size = Vector2.new(400, 346), -- Size of the gui
    can_resize = false, -- true or false
})

local Maintab = window:AddTab("Main")

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

    local bb = game:service'VirtualUser'
    game:service'Players'.LocalPlayer.Idled:connect(function()
        bb:CaptureController()
        bb:ClickButton2(Vector2.new())
        ab.Text = "Roblox tried kicking you, but I didn’t let them!"
        wait(2)
        ab.Text = "Status : Active"
    end)
end)

Maintab:AddButton("Destroy Ad teleport", function()
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

Killtab:AddSwitch("Auto Kill", function(bool)
    local teleportHeadsToRightHand = bool
    while teleportHeadsToRightHand do
        for _, player in pairs(game:GetService("Players"):GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("Head") then
                if not whitelist[player.Name] then
                    local head = player.Character.Head
                    local rightHand = game.Players.LocalPlayer.Character.RightHand
                    head.CFrame = rightHand.CFrame * CFrame.new(0, 0, 0)
                end
            end
        end
        wait(0.1)
    end
end)

Killtab:AddTextBox("Whitelist", function(text)
    whitelist[text] = true
end)

Killtab:AddButton("Clear Whitelist", function()
    whitelist = {}
end)

local targetPlayerName = ""
local teleporting = false

Killtab:AddTextBox("Write Player Name to kill", function(text)
    targetPlayerName = text
end)

Killtab:AddSwitch("Kill Player", function(bool)
    teleporting = bool
    while teleporting do
        if targetPlayerName ~= "" then
            local targetPlayer = game:GetService("Players"):FindFirstChild(targetPlayerName)
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("Head") then
                local head = targetPlayer.Character.Head
                local rightHand = game.Players.LocalPlayer.Character.RightHand
                head.CFrame = rightHand.CFrame * CFrame.new(0, 0, 0)
            end
        end
        wait(0.1)
    end
end)

Killtab:AddButton("Clear Player", function()
    targetPlayerName = ""
end)

Killtab:AddButton("Speed Punch", function()
    local player = game.Players.LocalPlayer
    local punch = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch")
    if punch and punch:FindFirstChild("attackTime") then
        punch.attackTime.Value = 0
    end
end)

local AutoPunchToggle = false

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
                wait(0.01)
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

-- Auto Farm Tab
local AutoFarmTab = window:AddTab("Auto Farm")
local AutoWeightToggle = false
local AutoPushupsToggle = false
local SitupsToggle = false
local MuscleKingFarmToggle = false

-- Helper functions for auto actions
local function AutoWeight()
    spawn(function()
        while AutoWeightToggle do
            local player = game.Players.LocalPlayer
            local weightTool = player.Backpack:FindFirstChild("Weight") or player.Character:FindFirstChild("Weight")
            if weightTool then
                player.Character.Humanoid:EquipTool(weightTool)
                weightTool:Activate()
            end
            wait(0.1)
        end
    end)
end

AutoFarmTab:AddSwitch("Auto Weight", function(State)
    AutoWeightToggle = State
    if AutoWeightToggle then
        AutoWeight()
    end
end)

-- More farm methods can be added here in the future

window:Show()
