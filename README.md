
]]
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local tabVisible = false
local flyEnabled = false
local speedEnabled = false
local flingEnabled = false
local flyConnection
local flingConnection
local bodyVelocity
local bodyAngularVelocity
local flingBodyVelocity
local flingBodyAngularVelocity

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "YoukiGUI"
screenGui.Parent = playerGui
screenGui.ResetOnSpawn = false

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 300, 0, 400)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 25)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = mainFrame

local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "Title"
titleLabel.Size = UDim2.new(1, 0, 0, 60)
titleLabel.Position = UDim2.new(0, 0, 0, 10)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "Youki"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = mainFrame

local flyFrame = Instance.new("Frame")
flyFrame.Size = UDim2.new(0.9, 0, 0, 50)
flyFrame.Position = UDim2.new(0.05, 0, 0, 80)
flyFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
flyFrame.BorderSizePixel = 0
flyFrame.Parent = mainFrame

local flyCorner = Instance.new("UICorner")
flyCorner.CornerRadius = UDim.new(0, 8)
flyCorner.Parent = flyFrame

local flyLabel = Instance.new("TextLabel")
flyLabel.Size = UDim2.new(0.7, 0, 1, 0)
flyLabel.Position = UDim2.new(0, 10, 0, 0)
flyLabel.BackgroundTransparency = 1
flyLabel.Text = "FreezeAir"
flyLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
flyLabel.TextScaled = true
flyLabel.Font = Enum.Font.Gotham
flyLabel.TextXAlignment = Enum.TextXAlignment.Left
flyLabel.Parent = flyFrame

local flyToggle = Instance.new("TextButton")
flyToggle.Size = UDim2.new(0, 40, 0, 30)
flyToggle.Position = UDim2.new(1, -50, 0.5, -15)
flyToggle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
flyToggle.Text = ""
flyToggle.BorderSizePixel = 0
flyToggle.Parent = flyFrame

local flyToggleCorner = Instance.new("UICorner")
flyToggleCorner.CornerRadius = UDim.new(0, 15)
flyToggleCorner.Parent = flyToggle

local speedFrame = Instance.new("Frame")
speedFrame.Size = UDim2.new(0.9, 0, 0, 50)
speedFrame.Position = UDim2.new(0.05, 0, 0, 140)
speedFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
speedFrame.BorderSizePixel = 0
speedFrame.Parent = mainFrame

local speedCorner = Instance.new("UICorner")
speedCorner.CornerRadius = UDim.new(0, 8)
speedCorner.Parent = speedFrame

local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(0.7, 0, 1, 0)
speedLabel.Position = UDim2.new(0, 10, 0, 0)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Speed (35)"
speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
speedLabel.TextScaled = true
speedLabel.Font = Enum.Font.Gotham
speedLabel.TextXAlignment = Enum.TextXAlignment.Left
speedLabel.Parent = speedFrame

local speedToggle = Instance.new("TextButton")
speedToggle.Size = UDim2.new(0, 40, 0, 30)
speedToggle.Position = UDim2.new(1, -50, 0.5, -15)
speedToggle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
speedToggle.Text = ""
speedToggle.BorderSizePixel = 0
speedToggle.Parent = speedFrame

local speedToggleCorner = Instance.new("UICorner")
speedToggleCorner.CornerRadius = UDim.new(0, 15)
speedToggleCorner.Parent = speedToggle

local flingFrame = Instance.new("Frame")
flingFrame.Size = UDim2.new(0.9, 0, 0, 50)
flingFrame.Position = UDim2.new(0.05, 0, 0, 200)
flingFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
flingFrame.BorderSizePixel = 0
flingFrame.Parent = mainFrame

local flingCorner = Instance.new("UICorner")
flingCorner.CornerRadius = UDim.new(0, 8)
flingCorner.Parent = flingFrame

local flingLabel = Instance.new("TextLabel")
flingLabel.Size = UDim2.new(0.7, 0, 1, 0)
flingLabel.Position = UDim2.new(0, 10, 0, 0)
flingLabel.BackgroundTransparency = 1
flingLabel.Text = "Super Jump"
flingLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
flingLabel.TextScaled = true
flingLabel.Font = Enum.Font.Gotham
flingLabel.TextXAlignment = Enum.TextXAlignment.Left
flingLabel.Parent = flingFrame

local flingToggle = Instance.new("TextButton")
flingToggle.Size = UDim2.new(0, 40, 0, 30)
flingToggle.Position = UDim2.new(1, -50, 0.5, -15)
flingToggle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
flingToggle.Text = ""
flingToggle.BorderSizePixel = 0
flingToggle.Parent = flingFrame

local flingToggleCorner = Instance.new("UICorner")
flingToggleCorner.CornerRadius = UDim.new(0, 15)
flingToggleCorner.Parent = flingToggle

local tabButton = Instance.new("TextButton")
tabButton.Name = "TabButton"
tabButton.Size = UDim2.new(0, 120, 0, 50)
tabButton.Position = UDim2.new(0, 20, 1, -70)
tabButton.BackgroundColor3 = Color3.fromRGB(45, 45, 65)
tabButton.Text = "Youki"
tabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
tabButton.TextScaled = true
tabButton.Font = Enum.Font.GothamBold
tabButton.BorderSizePixel = 0
tabButton.Parent = screenGui

local tabCorner = Instance.new("UICorner")
tabCorner.CornerRadius = UDim.new(0, 25)
tabCorner.Parent = tabButton

local buttonTween = TweenService:Create(tabButton, TweenInfo.new(0.3, Enum.EasingStyle.Back), {Size = UDim2.new(0, 130, 0, 55)})
local buttonTweenOut = TweenService:Create(tabButton, TweenInfo.new(0.3, Enum.EasingStyle.Back), {Size = UDim2.new(0, 120, 0, 50)})

local function toggleFly()
    flyEnabled = not flyEnabled
    local character = player.Character
    if not character then return end
    
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end
    
    if flyEnabled then
        flyToggle.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.Parent = humanoidRootPart
        
        bodyAngularVelocity = Instance.new("BodyAngularVelocity")
        bodyAngularVelocity.MaxTorque = Vector3.new(4000, 4000, 4000)
        bodyAngularVelocity.AngularVelocity = Vector3.new(0, 0, 0)
        bodyAngularVelocity.Parent = humanoidRootPart
        
        flyConnection = RunService.Heartbeat:Connect(function()
            if humanoidRootPart and bodyVelocity and bodyAngularVelocity then
                local camera = workspace.CurrentCamera
                local velocity = Vector3.new(0, 0, 0)
                
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                    velocity = velocity + camera.CFrame.LookVector * 50
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                    velocity = velocity - camera.CFrame.LookVector * 50
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                    velocity = velocity - camera.CFrame.RightVector * 50
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                    velocity = velocity + camera.CFrame.RightVector * 50
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                    velocity = velocity + Vector3.new(0, 50, 0)
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                    velocity = velocity - Vector3.new(0, 50, 0)
                end
                
                bodyVelocity.Velocity = velocity
                bodyAngularVelocity.AngularVelocity = Vector3.new(0, 0, 0)
            end
        end)
    else
        flyToggle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        if flyConnection then
            flyConnection:Disconnect()
            flyConnection = nil
        end
        if bodyVelocity then
            bodyVelocity:Destroy()
            bodyVelocity = nil
        end
        if bodyAngularVelocity then
            bodyAngularVelocity:Destroy()
            bodyAngularVelocity = nil
        end
    end
end

local function toggleSpeed()
    speedEnabled = not speedEnabled
    local character = player.Character
    if not character then return end
    
    local humanoid = character:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    if speedEnabled then
        speedToggle.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        humanoid.WalkSpeed = 35
    else
        speedToggle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        humanoid.WalkSpeed = 16
    end
end

local function toggleFling()
    flingEnabled = not flingEnabled
    local character = player.Character
    if not character then return end
    
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end
    
    if flingEnabled then
        flingToggle.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(40000, 40000, 40000)
        bodyVelocity.Velocity = Vector3.new(0, 150, 0)
        bodyVelocity.Parent = humanoidRootPart
        
        game:GetService("Debris"):AddItem(bodyVelocity, 1)
    else
        flingToggle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    end
end

flyToggle.MouseButton1Click:Connect(toggleFly)
speedToggle.MouseButton1Click:Connect(toggleSpeed)
flingToggle.MouseButton1Click:Connect(toggleFling)

tabButton.MouseButton1Click:Connect(function()
    tabVisible = not tabVisible
    if tabVisible then
        mainFrame.Visible = true
        buttonTween:Play()
        tabButton.Text = "Youki ✓"
    else
        mainFrame.Visible = false
        buttonTweenOut:Play()
        tabButton.Text = "Youki"
    end
end)
