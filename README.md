# Pizza-overload
This will find bunches of pizza and also non-visual
-- =========================================================
-- OBLIVION HUB V12: FINAL JUDGMENT (ULTRA COOL EDITION)
-- THE PIZZA SINGULARITY PROTOCOL
-- =========================================================

local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")

local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- [1] STYLIZED UI SETUP
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "OblivionV12_Final"
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 0, 0, 0) -- Starts at zero for the "Pop" animation
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.BackgroundColor3 = Color3.fromRGB(5, 5, 10)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(255, 0, 80)
UIStroke.Thickness = 4
UIStroke.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 60)
Title.Text = "OBLIVION HUB: JUDGMENT"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 22
Title.BackgroundTransparency = 1
Title.Parent = MainFrame

-- [2] DRAMATIC LOADING BAR
local BarContainer = Instance.new("Frame")
BarContainer.Size = UDim2.new(0, 300, 0, 10)
BarContainer.Position = UDim2.new(0.5, -150, 0.7, 0)
BarContainer.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
BarContainer.Visible = false
BarContainer.Parent = MainFrame

local BarFill = Instance.new("Frame")
BarFill.Size = UDim2.new(0, 0, 1, 0)
BarFill.BackgroundColor3 = Color3.fromRGB(255, 0, 80)
BarFill.BorderSizePixel = 0
BarFill.Parent = BarContainer

local StatusText = Instance.new("TextLabel")
StatusText.Size = UDim2.new(1, 0, 0, 30)
StatusText.Position = UDim2.new(0, 0, -1.5, 0)
StatusText.BackgroundTransparency = 1
StatusText.TextColor3 = Color3.fromRGB(255, 0, 80)
StatusText.Font = Enum.Font.Code
StatusText.TextSize = 14
StatusText.Text = "WAITING FOR INPUT..."
StatusText.Parent = BarContainer

-- [3] THE OVERRIDE BUTTON
local DeployBtn = Instance.new("TextButton")
DeployBtn.Size = UDim2.new(0, 250, 0, 50)
DeployBtn.Position = UDim2.new(0.5, -125, 0.5, -25)
DeployBtn.Text = "INITIATE LAST RESORT"
DeployBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 80)
DeployBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
DeployBtn.Font = Enum.Font.GothamBold
DeployBtn.TextSize = 16
DeployBtn.Parent = MainFrame
Instance.new("UICorner", DeployBtn)

-- [4] COOL ANIMATION LOGIC
local function introAnimation()
    MainFrame:TweenSize(UDim2.new(0, 400, 0, 250), "Out", "Elastic", 1.5)
    task.wait(1)
    -- Pulse the border
    task.spawn(function()
        while true do
            TweenService:Create(UIStroke, TweenInfo.new(1), {Thickness = 8, Transparency = 0.5}):Play()
            task.wait(1)
            TweenService:Create(UIStroke, TweenInfo.new(1), {Thickness = 4, Transparency = 0}):Play()
            task.wait(1)
        end
    end)
end

-- [5] THE CHAOTIC PAYLOAD
local function startChaos()
    for i = 1, 500 do
        local p = Instance.new("Part")
        p.Size = Vector3.new(40, 2, 40)
        p.Position = Player.Character.HumanoidRootPart.Position + Vector3.new(math.random(-100, 100), 200, math.random(-100, 100))
        p.Velocity = Vector3.new(math.random(-50, 50), -300, math.random(-50, 50))
        p.Color = Color3.fromRGB(255, 150, 0)
        p.Material = Enum.Material.Neon
        p.Parent = workspace
        
        local d = Instance.new("Decal")
        d.Texture = "rbxassetid://603426749"
        d.Face = Enum.NormalId.Top
        d.Parent = p
        
        task.wait(0.01)
    end
end

-- [6] EXECUTION
DeployBtn.MouseButton1Click:Connect(function()
    DeployBtn.Visible = false
    BarContainer.Visible = true
    
    local countdown = 60
    local shakeIntensity = 0
    
    -- Loading Bar Tween
    TweenService:Create(BarFill, TweenInfo.new(60, Enum.EasingStyle.Linear), {Size = UDim2.new(1, 0, 1, 0)}):Play()
    
    -- The "Drama" Loop
    local connection = RunService.RenderStepped:Connect(function()
        MainFrame.Position = UDim2.new(0.5, math.random(-shakeIntensity, shakeIntensity), 0.5, math.random(-shakeIntensity, shakeIntensity))
        MainFrame.Rotation = math.random(-shakeIntensity/2, shakeIntensity/2)
    end)
    
    for i = countdown, 0, -1 do
        StatusText.Text = "CHAOS SYNCHRONIZATION: " .. i .. "s"
        shakeIntensity = shakeIntensity + 0.2 -- Shakes harder as it gets closer
        if i == 10 then StatusText.TextColor3 = Color3.fromRGB(255, 255, 255) end
        task.wait(1)
    end
    
    StatusText.Text = "TERMINATING SERVER SANITY..."
    task.wait(1)
    connection:Disconnect()
    startChaos()
end)

introAnimation()
