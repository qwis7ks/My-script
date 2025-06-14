-- 🚀 Professional Movement Script with Advanced GUI
-- تم إنشاؤه خصيصاً لك!

print("🔥 Loading Professional Movement Script...")

-- المتغيرات الأساسية
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

-- متغيرات التحكم
local isFlying = false
local currentSpeed = 16
local currentJump = 50
local bodyVelocity = nil
local bodyAngularVelocity = nil

-- إنشاء الواجهة الرئيسية
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ProfessionalMovementGUI"
screenGui.Parent = player.PlayerGui
screenGui.ResetOnSpawn = false

-- الإطار الرئيسي
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Parent = screenGui
mainFrame.Size = UDim2.new(0, 350, 0, 450)
mainFrame.Position = UDim2.new(0.5, -175, 0.5, -225)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true

-- تأثير الزوايا المدورة
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 15)
corner.Parent = mainFrame

-- تأثير الظل
local shadow = Instance.new("Frame")
shadow.Name = "Shadow"
shadow.Parent = screenGui
shadow.Size = mainFrame.Size + UDim2.new(0, 10, 0, 10)
shadow.Position = mainFrame.Position - UDim2.new(0, 5, 0, 5)
shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
shadow.BackgroundTransparency = 0.8
shadow.ZIndex = mainFrame.ZIndex - 1

local shadowCorner = Instance.new("UICorner")
shadowCorner.CornerRadius = UDim.new(0, 15)
shadowCorner.Parent = shadow

-- العنوان
local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "TitleLabel"
titleLabel.Parent = mainFrame
titleLabel.Size = UDim2.new(1, 0, 0, 50)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundColor3 = Color3.fromRGB(45, 45, 65)
titleLabel.BorderSizePixel = 0
titleLabel.Text = "🚀 Professional Movement"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.GothamBold

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 15)
titleCorner.Parent = titleLabel

-- خط فاصل
local separator = Instance.new("Frame")
separator.Parent = mainFrame
separator.Size = UDim2.new(0.9, 0, 0, 2)
separator.Position = UDim2.new(0.05, 0, 0, 60)
separator.BackgroundColor3 = Color3.fromRGB(100, 100, 120)
separator.BorderSizePixel = 0

-- قسم السرعة
local speedSection = Instance.new("Frame")
speedSection.Parent = mainFrame
speedSection.Size = UDim2.new(0.9, 0, 0, 80)
speedSection.Position = UDim2.new(0.05, 0, 0, 80)
speedSection.BackgroundTransparency = 1

local speedLabel = Instance.new("TextLabel")
speedLabel.Parent = speedSection
speedLabel.Size = UDim2.new(1, 0, 0, 25)
speedLabel.Position = UDim2.new(0, 0, 0, 0)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "🏃 Walk Speed: " .. currentSpeed
speedLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
speedLabel.TextScaled = true
speedLabel.Font = Enum.Font.Gotham
speedLabel.TextXAlignment = Enum.TextXAlignment.Left

local speedSlider = Instance.new("Frame")
speedSlider.Parent = speedSection
speedSlider.Size = UDim2.new(1, 0, 0, 20)
speedSlider.Position = UDim2.new(0, 0, 0, 30)
speedSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
speedSlider.BorderSizePixel = 0

local speedSliderCorner = Instance.new("UICorner")
speedSliderCorner.CornerRadius = UDim.new(0, 10)
speedSliderCorner.Parent = speedSlider

local speedHandle = Instance.new("TextButton")
speedHandle.Parent = speedSlider
speedHandle.Size = UDim2.new(0, 20, 1, 0)
speedHandle.Position = UDim2.new(0, 0, 0, 0)
speedHandle.BackgroundColor3 = Color3.fromRGB(100, 150, 255)
speedHandle.BorderSizePixel = 0
speedHandle.Text = ""

local speedHandleCorner = Instance.new("UICorner")
speedHandleCorner.CornerRadius = UDim.new(0, 10)
speedHandleCorner.Parent = speedHandle

-- قسم القفز
local jumpSection = Instance.new("Frame")
jumpSection.Parent = mainFrame
jumpSection.Size = UDim2.new(0.9, 0, 0, 80)
jumpSection.Position = UDim2.new(0.05, 0, 0, 180)
jumpSection.BackgroundTransparency = 1

local jumpLabel = Instance.new("TextLabel")
jumpLabel.Parent = jumpSection
jumpLabel.Size = UDim2.new(1, 0, 0, 25)
jumpLabel.Position = UDim2.new(0, 0, 0, 0)
jumpLabel.BackgroundTransparency = 1
jumpLabel.Text = "🦘 Jump Power: " .. currentJump
jumpLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
jumpLabel.TextScaled = true
jumpLabel.Font = Enum.Font.Gotham
jumpLabel.TextXAlignment = Enum.TextXAlignment.Left

local jumpSlider = Instance.new("Frame")
jumpSlider.Parent = jumpSection
jumpSlider.Size = UDim2.new(1, 0, 0, 20)
jumpSlider.Position = UDim2.new(0, 0, 0, 30)
jumpSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
jumpSlider.BorderSizePixel = 0

local jumpSliderCorner = Instance.new("UICorner")
jumpSliderCorner.CornerRadius = UDim.new(0, 10)
jumpSliderCorner.Parent = jumpSlider

local jumpHandle = Instance.new("TextButton")
jumpHandle.Parent = jumpSlider
jumpHandle.Size = UDim2.new(0, 20, 1, 0)
jumpHandle.Position = UDim2.new(0, 0, 0, 0)
jumpHandle.BackgroundColor3 = Color3.fromRGB(100, 255, 150)
jumpHandle.BorderSizePixel = 0
jumpHandle.Text = ""

local jumpHandleCorner = Instance.new("UICorner")
jumpHandleCorner.CornerRadius = UDim.new(0, 10)
jumpHandleCorner.Parent = jumpHandle

-- أزرار التحكم
local buttonsFrame = Instance.new("Frame")
buttonsFrame.Parent = mainFrame
buttonsFrame.Size = UDim2.new(0.9, 0, 0, 120)
buttonsFrame.Position = UDim2.new(0.05, 0, 0, 280)
buttonsFrame.BackgroundTransparency = 1

-- زر الطيران
local flyButton = Instance.new("TextButton")
flyButton.Parent = buttonsFrame
flyButton.Size = UDim2.new(1, 0, 0, 35)
flyButton.Position = UDim2.new(0, 0, 0, 0)
flyButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
flyButton.BorderSizePixel = 0
flyButton.Text = "✈️ Toggle Fly"
flyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
flyButton.TextScaled = true
flyButton.Font = Enum.Font.GothamBold

local flyButtonCorner = Instance.new("UICorner")
flyButtonCorner.CornerRadius = UDim.new(0, 8)
flyButtonCorner.Parent = flyButton

-- زر إعادة التعيين
local resetButton = Instance.new("TextButton")
resetButton.Parent = buttonsFrame
resetButton.Size = UDim2.new(0.48, 0, 0, 35)
resetButton.Position = UDim2.new(0, 0, 0, 45)
resetButton.BackgroundColor3 = Color3.fromRGB(255, 200, 100)
resetButton.BorderSizePixel = 0
resetButton.Text = "🔄 Reset"
resetButton.TextColor3 = Color3.fromRGB(50, 50, 50)
resetButton.TextScaled = true
resetButton.Font = Enum.Font.GothamBold

local resetButtonCorner = Instance.new("UICorner")
resetButtonCorner.CornerRadius = UDim.new(0, 8)
resetButtonCorner.Parent = resetButton

-- زر الإغلاق
local closeButton = Instance.new("TextButton")
closeButton.Parent = buttonsFrame
closeButton.Size = UDim2.new(0.48, 0, 0, 35)
closeButton.Position = UDim2.new(0.52, 0, 0, 45)
closeButton.BackgroundColor3 = Color3.fromRGB(200, 100, 100)
closeButton.BorderSizePixel = 0
closeButton.Text = "❌ Close"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextScaled = true
closeButton.Font = Enum.Font.GothamBold

local closeButtonCorner = Instance.new("UICorner")
closeButtonCorner.CornerRadius = UDim.new(0, 8)
closeButtonCorner.Parent = closeButton

-- معلومات المطور
local infoLabel = Instance.new("TextLabel")
infoLabel.Parent = mainFrame
infoLabel.Size = UDim2.new(1, 0, 0, 30)
infoLabel.Position = UDim2.new(0, 0, 1, -30)
infoLabel.BackgroundTransparency = 1
infoLabel.Text = "Made with ❤️ for you"
infoLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
infoLabel.TextScaled = true
infoLabel.Font = Enum.Font.Gotham

-- الوظائف
local function updateSpeed(value)
    currentSpeed = math.floor(value * 184 + 16) -- من 16 إلى 200
    humanoid.WalkSpeed = currentSpeed
    speedLabel.Text = "🏃 Walk Speed: " .. currentSpeed
end

local function updateJump(value)
    currentJump = math.floor(value * 450 + 50) -- من 50 إلى 500
    humanoid.JumpPower = currentJump
    jumpLabel.Text = "🦘 Jump Power: " .. currentJump
end

local function toggleFly()
    isFlying = not isFlying
    
    if isFlying then
        -- تفعيل الطيران
        bodyVelocity = Instance.new("BodyVelocity")
        bodyAngularVelocity = Instance.new("BodyAngularVelocity")
        
        bodyVelocity.Parent = rootPart
        bodyAngularVelocity.Parent = rootPart
        
        bodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        
        bodyAngularVelocity.MaxTorque = Vector3.new(4000, 4000, 4000)
        bodyAngularVelocity.AngularVelocity = Vector3.new(0, 0, 0)
        
        flyButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
        flyButton.Text = "🚁 Flying (ON)"
        
        print("✈️ Fly mode activated!")
    else
        -- إيقاف الطيران
        if bodyVelocity then bodyVelocity:Destroy() end
        if bodyAngularVelocity then bodyAngularVelocity:Destroy() end
        
        flyButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
        flyButton.Text = "✈️ Toggle Fly"
        
        print("🚶 Fly mode deactivated!")
    end
end

local function resetValues()
    currentSpeed = 16
    currentJump = 50
    humanoid.WalkSpeed = currentSpeed
    humanoid.JumpPower = currentJump
    
    speedLabel.Text = "🏃 Walk Speed: " .. currentSpeed
    jumpLabel.Text = "🦘 Jump Power: " .. currentJump
    
    speedHandle.Position = UDim2.new(0, 0, 0, 0)
    jumpHandle.Position = UDim2.new(0, 0, 0, 0)
    
    if isFlying then
        toggleFly()
    end
    
    print("🔄 Values reset to default!")
end

-- أحداث التفاعل مع السلايدر
local speedDragging = false
local jumpDragging = false

speedHandle.MouseButton1Down:Connect(function()
    speedDragging = true
end)

jumpHandle.MouseButton1Down:Connect(function()
    jumpDragging = true
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        speedDragging = false
        jumpDragging = false
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        if speedDragging then
            local mouse = Players.LocalPlayer:GetMouse()
            local sliderPos = speedSlider.AbsolutePosition.X
            local sliderSize = speedSlider.AbsoluteSize.X
            local mouseX = mouse.X
            
            local relativeX = math.clamp((mouseX - sliderPos) / sliderSize, 0, 1)
            speedHandle.Position = UDim2.new(relativeX, -10, 0, 0)
            updateSpeed(relativeX)
        end
        
        if jumpDragging then
            local mouse = Players.LocalPlayer:GetMouse()
            local sliderPos = jumpSlider.AbsolutePosition.X
            local sliderSize = jumpSlider.AbsoluteSize.X
            local mouseX = mouse.X
            
            local relativeX = math.clamp((mouseX - sliderPos) / sliderSize, 0, 1)
            jumpHandle.Position = UDim2.new(relativeX, -10, 0, 0)
            updateJump(relativeX)
        end
    end
end)

-- التحكم بالطيران
local function updateFlyMovement()
    if isFlying and bodyVelocity then
        local camera = workspace.CurrentCamera
        local moveVector = Vector3.new(0, 0, 0)
        
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
            moveVector = moveVector + camera.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
            moveVector = moveVector - camera.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
            moveVector = moveVector - camera.CFrame.RightVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
            moveVector = moveVector + camera.CFrame.RightVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
            moveVector = moveVector + Vector3.new(0, 1, 0)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
            moveVector = moveVector - Vector3.new(0, 1, 0)
        end
        
        bodyVelocity.Velocity = moveVector * (currentSpeed * 2)
    end
end

-- ربط الأحداث
flyButton.MouseButton1Click:Connect(toggleFly)
resetButton.MouseButton1Click:Connect(resetValues)
closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
    if bodyVelocity then bodyVelocity:Destroy() end
    if bodyAngularVelocity then bodyAngularVelocity:Destroy() end
    print("👋 GUI closed!")
end)

-- تشغيل حلقة الطيران
game:GetService("RunService").Heartbeat:Connect(updateFlyMovement)

-- تأثيرات بصرية للأزرار
local function addHoverEffect(button, normalColor, hoverColor)
    button.MouseEnter:Connect(function()
        local tween = TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = hoverColor})
        tween:Play()
    end)
    
    button.MouseLeave:Connect(function()
        local tween = TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = normalColor})
        tween:Play()
    end)
end

addHoverEffect(flyButton, Color3.fromRGB(255, 100, 100), Color3.fromRGB(255, 130, 130))
addHoverEffect(resetButton, Color3.fromRGB(255, 200, 100), Color3.fromRGB(255, 220, 130))
addHoverEffect(closeButton, Color3.fromRGB(200, 100, 100), Color3.fromRGB(220, 130, 130))

-- رسالة النجاح
print("🎉 Professional Movement Script loaded successfully!")
print("🎮 GUI is ready to use!")
print("✨ Enjoy your enhanced movement experience!")

-- تحديث الشخصية عند إعادة الإحياء
player.CharacterAdded:Connect(function(newCharacter)
    character = newCharacter
    humanoid = character:WaitForChild("Humanoid")
    rootPart = character:WaitForChild("HumanoidRootPart")
    
    -- إعادة تطبيق الإعدادات
    humanoid.WalkSpeed = currentSpeed
    humanoid.JumpPower = currentJump
    
    print("🔄 Script reconnected to new character!")
end)
