repeat task.wait() until game:IsLoaded()

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
local GuiParent = (gethui and gethui()) or CoreGui

local Theme = {
    Background = Color3.fromRGB(12, 10, 22),
    PanelBg = Color3.fromRGB(19, 15, 34),
    AccentPurple = Color3.fromRGB(112, 64, 210),
    AccentPink = Color3.fromRGB(255, 110, 185),
    TextMain = Color3.fromRGB(255, 255, 255),
    TextMuted = Color3.fromRGB(165, 158, 188),
    ButtonBg = Color3.fromRGB(30, 24, 54),
    Success = Color3.fromRGB(46, 204, 113)
}

local function new(class, props, parent)
    local obj = Instance.new(class)

    for key, value in pairs(props or {}) do
        obj[key] = value
    end

    if parent then
        obj.Parent = parent
    end

    return obj
end

local function corner(obj, radius)
    new("UICorner", {
        CornerRadius = UDim.new(0, radius)
    }, obj)
end

local function stroke(obj, color, thickness)
    new("UIStroke", {
        Color = color,
        Thickness = thickness or 1,
        ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    }, obj)
end

local function parentGui(gui)
    local success = pcall(function()
        gui.Parent = GuiParent
    end)

    if not success then
        gui.Parent = PlayerGui
    end
end

local function destroyOldGui(name)
    pcall(function()
        local old = GuiParent:FindFirstChild(name)
        if old then
            old:Destroy()
        end
    end)

    pcall(function()
        local old = PlayerGui:FindFirstChild(name)
        if old then
            old:Destroy()
        end
    end)
end

destroyOldGui("DanisUniversalHub")
destroyOldGui("DanisHubAutomationUI")
destroyOldGui("BatmanMainLauncher")

local function makeDraggable(frame, handle)
    handle = handle or frame

    local dragging = false
    local dragStart
    local startPosition

    handle.InputBegan:Connect(function(input)
        if input.UserInputType ~= Enum.UserInputType.MouseButton1
            and input.UserInputType ~= Enum.UserInputType.Touch then
            return
        end

        dragging = true
        dragStart = input.Position
        startPosition = frame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end)

    UserInputService.InputChanged:Connect(function(input)
        if not dragging then
            return
        end

        if input.UserInputType ~= Enum.UserInputType.MouseMovement
            and input.UserInputType ~= Enum.UserInputType.Touch then
            return
        end

        local delta = input.Position - dragStart

        frame.Position = UDim2.new(
            startPosition.X.Scale,
            startPosition.X.Offset + delta.X,
            startPosition.Y.Scale,
            startPosition.Y.Offset + delta.Y
        )
    end)
end

local launcher = new("ScreenGui", {
    Name = "BatmanMainLauncher",
    ResetOnSpawn = false,
    ZIndexBehavior = Enum.ZIndexBehavior.Sibling
})

parentGui(launcher)

local container = new("Frame", {
    Name = "Container",
    Size = UDim2.fromOffset(850, 420),
    Position = UDim2.new(0.5, -425, 0.5, -210),
    BackgroundTransparency = 1,
    Active = true
}, launcher)

makeDraggable(container)

new("UIListLayout", {
    FillDirection = Enum.FillDirection.Horizontal,
    SortOrder = Enum.SortOrder.LayoutOrder,
    Padding = UDim.new(0, 15),
    HorizontalAlignment = Enum.HorizontalAlignment.Center,
    VerticalAlignment = Enum.VerticalAlignment.Center
}, container)

local userPanel = new("Frame", {
    Name = "UserPanel",
    Size = UDim2.new(0, 220, 1, 0),
    BackgroundColor3 = Theme.PanelBg,
    LayoutOrder = 1
}, container)

corner(userPanel, 10)
stroke(userPanel, Theme.AccentPurple, 1)

new("TextLabel", {
    Size = UDim2.new(1, -20, 0, 40),
    Position = UDim2.fromOffset(15, 5),
    BackgroundTransparency = 1,
    Text = "User Info",
    Font = Enum.Font.GothamBold,
    TextSize = 16,
    TextColor3 = Theme.TextMain,
    TextXAlignment = Enum.TextXAlignment.Left
}, userPanel)

local avatarBox = new("Frame", {
    Size = UDim2.fromOffset(75, 75),
    Position = UDim2.new(0.5, -37, 0, 50),
    BackgroundColor3 = Theme.ButtonBg
}, userPanel)

corner(avatarBox, 12)
stroke(avatarBox, Theme.AccentPink, 1.5)

local avatar = new("ImageLabel", {
    Size = UDim2.new(1, -6, 1, -6),
    Position = UDim2.fromOffset(3, 3),
    BackgroundTransparency = 1,
    Image = "rbxthumb://type=AvatarHeadShot&id=" .. LocalPlayer.UserId .. "&w=150&h=150"
}, avatarBox)

corner(avatar, 10)

new("TextLabel", {
    Size = UDim2.new(1, -20, 0, 25),
    Position = UDim2.fromOffset(10, 135),
    BackgroundTransparency = 1,
    Text = "Welcome, " .. LocalPlayer.DisplayName,
    Font = Enum.Font.GothamBold,
    TextSize = 14,
    TextColor3 = Theme.TextMain
}, userPanel)

local function infoRow(label, value, y)
    local row = new("Frame", {
        Size = UDim2.new(1, -30, 0, 18),
        Position = UDim2.fromOffset(15, y),
        BackgroundTransparency = 1
    }, userPanel)

    new("TextLabel", {
        Size = UDim2.new(0.45, 0, 1, 0),
        BackgroundTransparency = 1,
        Text = label,
        Font = Enum.Font.GothamSemibold,
        TextSize = 12,
        TextColor3 = Theme.TextMuted,
        TextXAlignment = Enum.TextXAlignment.Left
    }, row)

    new("TextLabel", {
        Size = UDim2.new(0.55, 0, 1, 0),
        Position = UDim2.new(0.45, 0, 0, 0),
        BackgroundTransparency = 1,
        Text = tostring(value),
        Font = Enum.Font.GothamBold,
        TextSize = 12,
        TextColor3 = Theme.AccentPink,
        TextXAlignment = Enum.TextXAlignment.Right
    }, row)
end

infoRow("Executor", identifyexecutor and identifyexecutor() or "Unknown", 175)
infoRow("Device", UserInputService.TouchEnabled and "Mobile" or "PC", 198)
infoRow("Support", "Full Support", 221)

local hwid = string.upper(string.sub(HttpService:GenerateGUID(false), 1, 18))

local hwidBox = new("Frame", {
    Size = UDim2.new(1, -30, 0, 30),
    Position = UDim2.fromOffset(15, 270),
    BackgroundColor3 = Theme.Background
}, userPanel)

corner(hwidBox, 6)
stroke(hwidBox, Theme.ButtonBg, 1)

local hwidText = new("TextLabel", {
    Size = UDim2.new(1, -50, 1, 0),
    Position = UDim2.fromOffset(8, 0),
    BackgroundTransparency = 1,
    Text = "HWID: HIDDEN",
    Font = Enum.Font.Code,
    TextSize = 10,
    TextColor3 = Theme.TextMuted,
    TextXAlignment = Enum.TextXAlignment.Left
}, hwidBox)

local copyBtn = new("TextButton", {
    Size = UDim2.fromOffset(38, 22),
    Position = UDim2.new(1, -43, 0.5, -11),
    BackgroundColor3 = Theme.ButtonBg,
    Text = "COPY",
    Font = Enum.Font.GothamBold,
    TextSize = 9,
    TextColor3 = Theme.AccentPink
}, hwidBox)

corner(copyBtn, 5)

copyBtn.MouseButton1Click:Connect(function()
    if setclipboard then
        setclipboard(hwid)
    end

    hwidText.Text = hwid
    hwidText.TextColor3 = Theme.Success

    task.wait(2)

    if hwidText.Parent then
        hwidText.Text = "HWID: HIDDEN"
        hwidText.TextColor3 = Theme.TextMuted
    end
end)

local clock = new("TextLabel", {
    Size = UDim2.new(1, -20, 0, 20),
    Position = UDim2.new(0, 10, 1, -50),
    BackgroundTransparency = 1,
    Text = "00:00:00",
    Font = Enum.Font.GothamBold,
    TextSize = 16,
    TextColor3 = Theme.AccentPink
}, userPanel)

local date = new("TextLabel", {
    Size = UDim2.new(1, -20, 0, 15),
    Position = UDim2.new(0, 10, 1, -28),
    BackgroundTransparency = 1,
    Text = "Jan 01, 1970",
    Font = Enum.Font.GothamSemibold,
    TextSize = 11,
    TextColor3 = Theme.TextMuted
}, userPanel)

task.spawn(function()
    while launcher.Parent do
        clock.Text = os.date("%I:%M:%S %p")
        date.Text = os.date("%b %d, %Y")
        task.wait(1)
    end
end)

local centerPanel = new("Frame", {
    Name = "CenterPanel",
    Size = UDim2.new(0, 360, 1, 0),
    BackgroundColor3 = Theme.PanelBg,
    LayoutOrder = 2
}, container)

corner(centerPanel, 10)
stroke(centerPanel, Theme.AccentPurple, 1.5)

local logo = new("TextLabel", {
    Size = UDim2.fromOffset(70, 70),
    Position = UDim2.new(0.5, -35, 0, 22),
    BackgroundColor3 = Theme.AccentPurple,
    Text = "D",
    Font = Enum.Font.GothamBlack,
    TextSize = 34,
    TextColor3 = Theme.TextMain
}, centerPanel)

corner(logo, 14)
stroke(logo, Theme.AccentPink, 1.5)

new("TextLabel", {
    Size = UDim2.new(1, 0, 0, 25),
    Position = UDim2.fromOffset(0, 110),
    BackgroundTransparency = 1,
    Text = "DANI'S UNIVERSAL",
    Font = Enum.Font.GothamBold,
    TextSize = 20,
    TextColor3 = Theme.TextMain
}, centerPanel)

new("TextLabel", {
    Size = UDim2.new(1, 0, 0, 15),
    Position = UDim2.fromOffset(0, 137),
    BackgroundTransparency = 1,
    Text = "Main Launcher",
    Font = Enum.Font.GothamSemibold,
    TextSize = 11,
    TextColor3 = Theme.TextMuted
}, centerPanel)

local launchBtn = new("TextButton", {
    Size = UDim2.new(1, -40, 0, 55),
    Position = UDim2.fromOffset(20, 175),
    BackgroundColor3 = Theme.AccentPurple,
    Text = "LAUNCH HUB",
    Font = Enum.Font.GothamBold,
    TextSize = 18,
    TextColor3 = Theme.TextMain,
    AutoButtonColor = false
}, centerPanel)

corner(launchBtn, 8)
stroke(launchBtn, Theme.AccentPink, 1)

launchBtn.MouseEnter:Connect(function()
    TweenService:Create(launchBtn, TweenInfo.new(0.2), {
        BackgroundColor3 = Theme.AccentPink
    }):Play()
end)

launchBtn.MouseLeave:Connect(function()
    TweenService:Create(launchBtn, TweenInfo.new(0.2), {
        BackgroundColor3 = Theme.AccentPurple
    }):Play()
end)

local joinBox = new("Frame", {
    Size = UDim2.new(1, -30, 0, 60),
    Position = UDim2.new(0, 15, 1, -75),
    BackgroundColor3 = Theme.Background
}, centerPanel)

corner(joinBox, 8)
stroke(joinBox, Theme.ButtonBg, 1)

new("TextLabel", {
    Size = UDim2.new(0.65, 0, 0, 20),
    Position = UDim2.fromOffset(12, 10),
    BackgroundTransparency = 1,
    Text = "Want instant updates?",
    Font = Enum.Font.GothamBold,
    TextSize = 13,
    TextColor3 = Theme.TextMain,
    TextXAlignment = Enum.TextXAlignment.Left
}, joinBox)

new("TextLabel", {
    Size = UDim2.new(0.65, 0, 0, 15),
    Position = UDim2.fromOffset(12, 31),
    BackgroundTransparency = 1,
    Text = "Join the official community.",
    Font = Enum.Font.GothamSemibold,
    TextSize = 10,
    TextColor3 = Theme.TextMuted,
    TextXAlignment = Enum.TextXAlignment.Left
}, joinBox)

local joinBtn = new("TextButton", {
    Size = UDim2.new(0.28, 0, 0, 34),
    Position = UDim2.new(0.7, 0, 0.5, -17),
    BackgroundColor3 = Theme.ButtonBg,
    Text = "Join",
    Font = Enum.Font.GothamBold,
    TextSize = 12,
    TextColor3 = Theme.AccentPink
}, joinBox)

corner(joinBtn, 6)
stroke(joinBtn, Theme.AccentPink, 1)

joinBtn.MouseButton1Click:Connect(function()
    local url = "https://discord.gg/jbdRrZ39f"

    if openurl then
        pcall(openurl, url)
    elseif setclipboard then
        setclipboard(url)
        joinBtn.Text = "Copied"
        task.wait(1)

        if joinBtn.Parent then
            joinBtn.Text = "Join"
        end
    end
end)

local changelog = new("Frame", {
    Name = "ChangelogPanel",
    Size = UDim2.new(0, 220, 1, 0),
    BackgroundColor3 = Theme.PanelBg,
    LayoutOrder = 3
}, container)

corner(changelog, 10)
stroke(changelog, Theme.AccentPurple, 1)

new("TextLabel", {
    Size = UDim2.new(1, -20, 0, 40),
    Position = UDim2.fromOffset(15, 5),
    BackgroundTransparency = 1,
    Text = "Changelog",
    Font = Enum.Font.GothamBold,
    TextSize = 16,
    TextColor3 = Theme.TextMain,
    TextXAlignment = Enum.TextXAlignment.Left
}, changelog)

local scroll = new("ScrollingFrame", {
    Size = UDim2.new(1, -20, 1, -60),
    Position = UDim2.fromOffset(10, 45),
    BackgroundTransparency = 1,
    ScrollBarThickness = 2,
    ScrollBarImageColor3 = Theme.AccentPurple,
    CanvasSize = UDim2.new(0, 0, 0, 0)
}, changelog)

local logLayout = new("UIListLayout", {
    SortOrder = Enum.SortOrder.LayoutOrder,
    Padding = UDim.new(0, 6)
}, scroll)

for index, text in ipairs({
    "Main launcher UI only",
    "Removed DANI'S HUB v2",
    "Removed all automation toggles",
    "Launch prints batmanScript"
}) do
    new("TextLabel", {
        Name = "LogEntry_" .. index,
        Size = UDim2.new(1, -10, 0, 24),
        BackgroundTransparency = 1,
        Text = "• " .. text,
        Font = Enum.Font.GothamMedium,
        TextSize = 11,
        TextColor3 = Theme.TextMuted,
        TextXAlignment = Enum.TextXAlignment.Left,
        TextWrapped = true
    }, scroll)
end

logLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    scroll.CanvasSize = UDim2.fromOffset(0, logLayout.AbsoluteContentSize.Y + 10)
end)

launchBtn.MouseButton1Click:Connect(function()


    print("BatmanScript Loaded")
	repeat task.wait() until game:IsLoaded()

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local Scripts = {
    {
        Name = "Drain The Lake",
        PlaceIds = {
            138381251771774,
            124786371598438
        },
        Url = "https://api.jnkie.com/api/v1/luascripts/public/5abf740d7d979b1aaf1a6066f1141de33640730ff873b927a451fe92a3308ca7/download"
    },

    {
        Name = "Mukbang Game",
        PlaceIds = {
            74188431054457
        },
        Url = "https://api.jnkie.com/api/v1/luascripts/public/e88ca53c5411b71906ac01239331111d2d77d67c491b6e54c29a0d73073c13b6/download"
    },

    {
        Name = "ABA",
        PlaceIds = {
            1458767429
        },
        Url = "https://api.jnkie.com/api/v1/luascripts/public/82abbc9b8b6f058db6bc946fc78cd6e8de55a73ec1fc41dac8fc1dc50f44233c/download"
    },

    {
        Name = "Secure The Airport",
        PlaceIds = {
            102054284786904,
			138145699008779
        },
        Url = "https://api.jnkie.com/api/v1/luascripts/public/9d9d741496f120035c8d857013d1496e35d72027729bf3c61f813e1a2aad864e/download"
    },

    {
        Name = "Miniwar",
        PlaceIds = {
            131346454575416
        },
        Url = "https://api.jnkie.com/api/v1/luascripts/public/a36c44850b10031e778ccdbb93f0fe2e341095ba424bc64fb40641a8277e3637/download"
    }
}

local ScriptData

for _, Data in ipairs(Scripts) do
    for _, PlaceId in ipairs(Data.PlaceIds or {}) do
        if game.PlaceId == PlaceId then
            ScriptData = Data
            break
        end
    end

    if ScriptData then
        break
    end

    for _, GameId in ipairs(Data.GameIds or {}) do
        if game.GameId == GameId then
            ScriptData = Data
            break
        end
    end

    if ScriptData then
        break
    end
end

if not ScriptData then
    LocalPlayer:Kick(
        "Unsupported game\n" ..
        "Place ID: " .. tostring(game.PlaceId) .. "\n" ..
        "Game ID: " .. tostring(game.GameId)
    )
    return
end

local Request = request or http_request or syn_request or fluxus_request

local Success, ErrorMessage = pcall(function()
    local Response = Request({
        Url = ScriptData.Url,
        Method = "GET"
    })

    if not Response or not Response.Success then
        error("Request failed: " .. tostring(Response and Response.StatusCode))
    end

    local LoadedScript, LoadError = loadstring(Response.Body)

    if not LoadedScript then
        error(LoadError)
    end

    LoadedScript()
end)

if not Success then
    warn("[Loader][" .. ScriptData.Name .. "] " .. tostring(ErrorMessage))
    LocalPlayer:Kick("Failed to load " .. ScriptData.Name)
end


    launcher:Destroy()
end)
