<img width="994" height="584" alt="image" src="https://github.com/user-attachments/assets/3423e3c1-f455-41f3-9d00-35d516f51f78" />
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
    Background = Color3.fromRGB(5, 5, 5),
    PanelBg = Color3.fromRGB(13, 13, 13),
    AccentDarkRed = Color3.fromRGB(110, 0, 0),
    AccentRed = Color3.fromRGB(225, 35, 35),
    TextMain = Color3.fromRGB(255, 255, 255),
    TextMuted = Color3.fromRGB(165, 165, 165),
    ButtonBg = Color3.fromRGB(24, 24, 24),
    Success = Color3.fromRGB(46, 204, 113),
    Gold = Color3.fromRGB(255, 205, 60)
}

local Scripts = {
    {
        Name = "Mukbang Game",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/e88ca53c5411b71906ac01239331111d2d77d67c491b6e54c29a0d73073c13b6/download",
        Image = "rbxthumb://type=Asset&id=7771801612&w=420&h=420",
        Desc = "Auto eat, Auto serve, Farm bites."
    },
    {
        Name = "Anime Battle Arena",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/82abbc9b8b6f058db6bc946fc78cd6e8de55a73ec1fc41dac8fc1dc50f44233c/download",
        Image = "rbxthumb://type=Asset&id=105470550715013&w=420&h=420",
        Desc = "Esp, Auto Blackflash, Auto skill."
    },
    {
        Name = "Drain The Lake (KEYLESS)",
        Url = "https://pastefy.app/IzVNuwBz/raw",
        Image = "rbxthumb://type=Asset&id=11846929740&w=420&h=420",
        Desc = "Auto farm, Auto sell, Anti-afk."
    },
    {
        Name = "Secure The Airport",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/9d9d741496f120035c8d857013d1496e35d72027729bf3c61f813e1a2aad864e/download",
        Image = "rbxthumb://type=Asset&id=9398081024&w=420&h=420",
        Desc = "Auto arrest, Auto kill, Auto luggage."
    },
    {
        Name = "MiniWar",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/a36c44850b10031e778ccdbb93f0fe2e341095ba424bc64fb40641a8277e3637/download",
        Image = "rbxthumb://type=Asset&id=87207067782717&w=420&h=420",
        Desc = "Auto farm, Auto buy, Auto collect."
    },
    {
        Name = "Animal Hospital",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/660c49b30e646e6cce128a72f0065d8b885686af47c2f1494c7baf14a8eccd50/download",
        Image = "rbxthumb://type=Asset&id=82940645739430&w=420&h=420",
        Desc = "Auto farm, Auto heal, Esp."
    },
    {
        Name = "MM2 (KEYLESS)",
        Url = "https://pastefy.app/0Ey9690b/raw",
        Image = "rbxthumb://type=Asset&id=838484753&w=420&h=420",
        Desc = "Auto coin, boost fps, Auto box."
    },
	{
        Name = "Night at the infirmary",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/0e5b3d262cdb2e27f6645a97f3bf4d06643e25d12730e637888644a66a73180d/download",
        Image = "rbxthumb://type=Asset&id=11891920271&w=420&h=420",
        Desc = "Auto checkin, Auto heal, esp."
    },
	{
        Name = "Home Alone (Anomaly)",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/ad7c82401cb2c897228819b03b8c30a902de114da2410b4cfb6675a8e7c141a8/download",
        Image = "rbxthumb://type=Asset&id=10794318086&w=420&h=420",
        Desc = "Esp, Inf sanity, Auto lobby"
    },
    {
        Name = "Cheating During Testing",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/540549516a368096569ea7a1d1a9e964c6d2cca3afdf3fae57db9c53a2373736/download",
        Image = "rbxthumb://type=Asset&id=22189922&w=420&h=420",
        Desc = "Auto Play, Esp, Auto Answer"
    },
    {
        Name = "RUNAWAYS [beta]",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/b775b19ded33ec24d70193f4cde6b7844930e2e9d4ec74fd02c683e5e12d2847/download",
        Image = "rbxthumb://type=Asset&id=14193174989&w=420&h=420",
        Desc = "Full Auto Play, Instant Win, Kill All"
    },
	{
        Name = "Anime Dice",
        Url = "https://api.jnkie.com/api/v1/luascripts/public/dd0b7e0ba5de73cfb04e1f77b67e13b5ae342517863ad1b4bef51cf76e87bdc1/download",
        Image = "rbxthumb://type=Asset&id=1119030529&w=420&h=420",
        Desc = "Full Auto Play, Instant Roll, Auto Collect"
    },
	{
        Name = "Steal an egg",
        Url = "https://pastefy.app/Z8dm0QE1/raw",
        Image = "rbxthumb://type=Asset&id=11622456609&w=420&h=420",
        Desc = "Auto Steal Egg, Insane Speed, Auto Hatch"
    }
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
    new("UICorner", { CornerRadius = UDim.new(0, radius) }, obj)
end

local function stroke(obj, color, thickness)
    new("UIStroke", {
        Color = color,
        Thickness = thickness or 1,
        ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    }, obj)
end

local function gradient(obj, colorSeq, rotation)
    new("UIGradient", { Color = colorSeq, Rotation = rotation or 0 }, obj)
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
        if old then old:Destroy() end
    end)
    pcall(function()
        local old = PlayerGui:FindFirstChild(name)
        if old then old:Destroy() end
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
        if not dragging then return end
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

local AUTORUN_SOURCE = [==[
repeat task.wait() until game:IsLoaded()

local SelectedSpec = __SPEC__
local AutorunSource = __SOURCE__

local function quote(value)
    return string.format("%q", tostring(value or ""))
end

local function buildPayload(spec)
    local payload = AutorunSource
    payload = payload:gsub("__SPEC__", function()
        return quote(spec)
    end, 1)
    payload = payload:gsub("__SOURCE__", function()
        return quote(AutorunSource)
    end, 1)
    return payload
end

local function normalizeSpec(spec)
    spec = tostring(spec or "")
    spec = spec:gsub("^%s+", ""):gsub("%s+$", "")

    local markdownUrl = spec:match("^%[.-%]%((https?://.-)%)$")
    if markdownUrl then
        spec = markdownUrl
    end

    return spec
end

local function extractUrl(spec)
    spec = normalizeSpec(spec)

    if spec:match("^https?://") then
        return spec
    end

    local url = spec:match("https?://[^\"'%s%)%]]+")
    if url then
        url = url:gsub("[,;]+$", "")
    end

    return url
end

local function getRequest()
    return request
        or http_request
        or syn_request
        or fluxus_request
        or (syn and syn.request)
end

local function executeSource(source)
    if type(source) ~= "string" or source == "" then
        return false, "Empty response body"
    end

    source = source:gsub("^\239\187\191", "")

    local fn, compileError = loadstring(source)
    if not fn then
        return false, compileError
    end

    local ok, runtimeError = pcall(fn)
    if not ok then
        return false, runtimeError
    end

    return true
end

local function fetchWithRequest(url)
    local Request = getRequest()
    if not Request then
        return nil, "Request function not supported"
    end

    local ok, response = pcall(function()
        return Request({
            Url = url,
            Method = "GET",
            Headers = {
                ["Accept"] = "*/*"
            }
        })
    end)

    if not ok then
        return nil, response
    end

    if type(response) == "string" then
        return response
    end

    if type(response) ~= "table" then
        return nil, "Invalid request response"
    end

    local body = response.Body or response.body or response.ResponseBody
    local status = tonumber(response.StatusCode or response.Status or response.status_code)

    if status and (status < 200 or status >= 400) then
        return nil, "HTTP " .. tostring(status)
    end

    if type(body) ~= "string" or body == "" then
        return nil, "Empty request body"
    end

    return body
end

local function fetchWithHttpGet(url)
    local ok, body = pcall(function()
        return game:HttpGet(url, true)
    end)

    if not ok then
        return nil, body
    end

    if type(body) ~= "string" or body == "" then
        return nil, "Empty HttpGet body"
    end

    return body
end

local function runUrl(url)
    -- Run URL exactly like: loadstring(game:HttpGet("URL"))()
    local ok, result = pcall(function()
        local source = game:HttpGet(url)
        local fn, compileError = loadstring(source)

        if not fn then
            error(compileError)
        end

        return fn()
    end)

    if ok then
        return true
    end

    return false, result
end

local function runAny(spec)
    spec = normalizeSpec(spec)
    if spec == "" then
        return false, "No URL/loader provided"
    end

    local url = extractUrl(spec)
    if url then
        local ok, err = runUrl(url)
        if ok then
            return true
        end

        if not spec:match("^https?://") then
            local directOk, directErr = executeSource(spec)
            if directOk then
                return true
            end
            return false, tostring(err) .. " | direct loader: " .. tostring(directErr)
        end

        return false, err
    end

    return executeSource(spec)
end

local QueueTeleport =
    queue_on_teleport
    or queueonteleport
    or (syn and syn.queue_on_teleport)

if QueueTeleport then
    pcall(function()
        QueueTeleport(buildPayload(SelectedSpec))
    end)
end

task.wait(1.5)

local ok, err = runAny(SelectedSpec)
if not ok then
    warn("[AutoTeleport] " .. tostring(err))
end
]==]

local function BuildTeleportPayload(spec)
    local payload = AUTORUN_SOURCE
    payload = payload:gsub("__SPEC__", function()
        return string.format("%q", tostring(spec or ""))
    end, 1)
    payload = payload:gsub("__SOURCE__", function()
        return string.format("%q", AUTORUN_SOURCE)
    end, 1)
    return payload
end

local function NormalizeSpec(spec)
    spec = tostring(spec or "")
    spec = spec:gsub("^%s+", ""):gsub("%s+$", "")

    local markdownUrl = spec:match("^%[.-%]%((https?://.-)%)$")
    if markdownUrl then
        spec = markdownUrl
    end

    return spec
end

local function ExtractUrl(spec)
    spec = NormalizeSpec(spec)

    if spec:match("^https?://") then
        return spec
    end

    local url = spec:match("https?://[^\"'%s%)%]]+")
    if url then
        url = url:gsub("[,;]+$", "")
    end

    return url
end

local function GetRequest()
    return request
        or http_request
        or syn_request
        or fluxus_request
        or (syn and syn.request)
end

local function ExecuteSource(source)
    if type(source) ~= "string" or source == "" then
        return false, "Empty response body"
    end

    source = source:gsub("^\239\187\191", "")

    local LoadedScript, LoadError = loadstring(source)
    if not LoadedScript then
        return false, LoadError
    end

    local ok, RuntimeError = pcall(LoadedScript)
    if not ok then
        return false, RuntimeError
    end

    return true
end

local function FetchWithRequest(url)
    local Request = GetRequest()
    if not Request then
        return nil, "Request function not supported"
    end

    local ok, Response = pcall(function()
        return Request({
            Url = url,
            Method = "GET",
            Headers = {
                ["Accept"] = "*/*"
            }
        })
    end)

    if not ok then
        return nil, Response
    end

    if type(Response) == "string" then
        return Response
    end

    if type(Response) ~= "table" then
        return nil, "Invalid request response"
    end

    local Body = Response.Body or Response.body or Response.ResponseBody
    local Status = tonumber(Response.StatusCode or Response.Status or Response.status_code)

    if Status and (Status < 200 or Status >= 400) then
        return nil, "HTTP " .. tostring(Status)
    end

    if type(Body) ~= "string" or Body == "" then
        return nil, "Empty request body"
    end

    return Body
end

local function FetchWithHttpGet(url)
    local ok, Body = pcall(function()
        return game:HttpGet(url, true)
    end)

    if not ok then
        return nil, Body
    end

    if type(Body) ~= "string" or Body == "" then
        return nil, "Empty HttpGet body"
    end

    return Body
end

local function RunUrl(url)
    -- Run URL exactly like: loadstring(game:HttpGet("URL"))()
    local ok, Result = pcall(function()
        local Source = game:HttpGet(url)
        local LoadedScript, LoadError = loadstring(Source)

        if not LoadedScript then
            error(LoadError)
        end

        return LoadedScript()
    end)

    if ok then
        return true
    end

    return false, Result
end

local function RunAny(spec)
    spec = NormalizeSpec(spec)
    if spec == "" then
        return false, "No URL/loader provided"
    end

    local url = ExtractUrl(spec)
    if url then
        local ok, err = RunUrl(url)
        if ok then
            return true
        end

        -- Full loader strings get one final direct-execution fallback.
        if not spec:match("^https?://") then
            local DirectOk, DirectError = ExecuteSource(spec)
            if DirectOk then
                return true
            end
            return false, tostring(err) .. " | direct loader: " .. tostring(DirectError)
        end

        return false, err
    end

    -- Also supports putting raw Lua loader/source directly in Url/Loader.
    return ExecuteSource(spec)
end

local function GetScriptSpec(ScriptData)
    return ScriptData.Loader or ScriptData.Url or ScriptData.Source
end

local function QueueSelectedScript(ScriptData)
    local QueueTeleport =
        queue_on_teleport
        or queueonteleport
        or (syn and syn.queue_on_teleport)

    if not QueueTeleport then
        warn("[Loader] Executor does not support queue_on_teleport")
        return false
    end

    local spec = GetScriptSpec(ScriptData)
    local Payload = BuildTeleportPayload(spec)

    local Success, ErrorMessage = pcall(function()
        QueueTeleport(Payload)
    end)

    if not Success then
        warn("[Loader][Queue] " .. tostring(ErrorMessage))
        return false
    end

    return true
end

local function RunScript(ScriptData)
    QueueSelectedScript(ScriptData)

    local spec = GetScriptSpec(ScriptData)
    local Success, ErrorMessage = RunAny(spec)

    if not Success then
        warn("[Loader][" .. ScriptData.Name .. "] " .. tostring(ErrorMessage))
        LocalPlayer:Kick("Failed to load " .. ScriptData.Name)
    end
end

local launcher = new("ScreenGui", {
    Name = "BatmanMainLauncher",
    ResetOnSpawn = false,
    ZIndexBehavior = Enum.ZIndexBehavior.Sibling
})
parentGui(launcher)

local container = new("Frame", {
    Name = "Container",
    Size = UDim2.fromOffset(850, 600),
    Position = UDim2.new(0.5, -425, 0.5, -300),
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

-- ===== USER PANEL =====
local userPanel = new("Frame", {
    Name = "UserPanel",
    Size = UDim2.new(0, 220, 0, 580),
    BackgroundColor3 = Theme.PanelBg,
    LayoutOrder = 1
}, container)

corner(userPanel, 10)
stroke(userPanel, Theme.AccentDarkRed, 1)

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
stroke(avatarBox, Theme.AccentRed, 1.5)

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
        TextColor3 = Theme.AccentRed,
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
    TextColor3 = Theme.AccentRed
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
    TextColor3 = Theme.AccentRed
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

-- ===== CENTER PANEL (iPhone style carousel, realistic bezel) =====
local centerPanel = new("Frame", {
    Name = "CenterPanel",
    Size = UDim2.new(0, 300, 0, 600),
    BackgroundColor3 = Color3.fromRGB(12, 12, 13),
    LayoutOrder = 2
}, container)

corner(centerPanel, 46)
stroke(centerPanel, Color3.fromRGB(55, 55, 58), 3)

-- inner bezel ring for depth
local bezelRing = new("Frame", {
    Size = UDim2.new(1, -6, 1, -6),
    Position = UDim2.fromOffset(3, 3),
    BackgroundTransparency = 1
}, centerPanel)
stroke(bezelRing, Color3.fromRGB(70, 70, 74), 1)
corner(bezelRing, 44)

local closeBtn = new("TextButton", {
    Name = "CloseButton",
    Size = UDim2.fromOffset(28, 28),
    Position = UDim2.new(1, -36, 0, 16),
    BackgroundColor3 = Color3.fromRGB(0, 0, 0),
    BackgroundTransparency = 0.25,
    Text = "X",
    Font = Enum.Font.GothamBold,
    TextSize = 13,
    TextColor3 = Color3.fromRGB(255, 255, 255),
    AutoButtonColor = false,
    ZIndex = 20
}, centerPanel)

corner(closeBtn, 14)
stroke(closeBtn, Color3.fromRGB(120, 120, 120), 1)

closeBtn.MouseButton1Click:Connect(function()
    launcher:Destroy()
end)

local SCREEN_RADIUS = 36

-- thicker top/bottom bezel like a real phone, thinner sides
local screen = new("Frame", {
    Size = UDim2.new(1, -20, 1, -34),
    Position = UDim2.fromOffset(10, 20),
    BackgroundColor3 = Color3.fromRGB(0, 0, 0),
    ClipsDescendants = true
}, centerPanel)
corner(screen, SCREEN_RADIUS)

local mapImage = new("ImageLabel", {
    Name = "MapImage",
    Size = UDim2.new(1, 0, 1, 0),
    BackgroundColor3 = Color3.fromRGB(15, 15, 18),
    ScaleType = Enum.ScaleType.Crop,
    ZIndex = 1
}, screen)
corner(mapImage, SCREEN_RADIUS)

local shadeTop = new("Frame", {
    Size = UDim2.new(1, 0, 0, 140),
    BackgroundColor3 = Color3.fromRGB(0, 0, 0),
    BackgroundTransparency = 1,
    ZIndex = 2
}, screen)
gradient(shadeTop, ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0,0,0)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0,0,0))
}), 90)
shadeTop.UIGradient.Transparency = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 0.05),
    NumberSequenceKeypoint.new(1, 1)
})

local shadeBottom = new("Frame", {
    Size = UDim2.new(1, 0, 0, 220),
    Position = UDim2.new(0, 0, 1, -220),
    BackgroundColor3 = Color3.fromRGB(0, 0, 0),
    BackgroundTransparency = 1,
    ZIndex = 2
}, screen)
gradient(shadeBottom, ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0,0,0)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0,0,0))
}), 90)
shadeBottom.UIGradient.Transparency = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 1),
    NumberSequenceKeypoint.new(1, 0.05)
})

local notch = new("Frame", {
    Size = UDim2.fromOffset(120, 24),
    Position = UDim2.new(0.5, -60, 0, 8),
    BackgroundColor3 = Color3.fromRGB(0, 0, 0),
    ZIndex = 10
}, screen)
corner(notch, 12)

local camDot = new("Frame", {
    Size = UDim2.fromOffset(8, 8),
    Position = UDim2.new(0.5, 34, 0, 16),
    BackgroundColor3 = Color3.fromRGB(25, 25, 30),
    ZIndex = 11
}, screen)
corner(camDot, 4)
stroke(camDot, Color3.fromRGB(60, 60, 65), 1)

local statusBar = new("TextLabel", {
    Size = UDim2.new(1, -30, 0, 20),
    Position = UDim2.fromOffset(15, 10),
    BackgroundTransparency = 1,
    Text = os.date("%I:%M"),
    Font = Enum.Font.GothamBold,
    TextSize = 13,
    TextColor3 = Color3.fromRGB(255, 255, 255),
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 11
}, screen)

task.spawn(function()
    while launcher.Parent do
        statusBar.Text = os.date("%I:%M")
        task.wait(15)
    end
end)

local titleLabel = new("TextLabel", {
    Size = UDim2.new(1, -30, 0, 32),
    Position = UDim2.fromOffset(15, 45),
    BackgroundTransparency = 1,
    Text = "",
    Font = Enum.Font.GothamBlack,
    TextSize = 24,
    TextColor3 = Theme.Gold,
    TextXAlignment = Enum.TextXAlignment.Left,
    TextStrokeTransparency = 0.4,
    TextStrokeColor3 = Color3.fromRGB(0, 0, 0),
    ZIndex = 3
}, screen)

local descLabel = new("TextLabel", {
    Size = UDim2.new(1, -30, 0, 40),
    Position = UDim2.fromOffset(15, 78),
    BackgroundTransparency = 1,
    Text = "",
    Font = Enum.Font.GothamMedium,
    TextSize = 12,
    TextColor3 = Theme.Gold,
    TextXAlignment = Enum.TextXAlignment.Left,
    TextWrapped = true,
    TextStrokeTransparency = 0.6,
    TextStrokeColor3 = Color3.fromRGB(0, 0, 0),
    ZIndex = 3
}, screen)

local pageDots = new("Frame", {
    Size = UDim2.new(1, 0, 0, 10),
    Position = UDim2.new(0, 0, 1, -100),
    BackgroundTransparency = 1,
    ZIndex = 3
}, screen)

new("UIListLayout", {
    FillDirection = Enum.FillDirection.Horizontal,
    HorizontalAlignment = Enum.HorizontalAlignment.Center,
    Padding = UDim.new(0, 6),
    SortOrder = Enum.SortOrder.LayoutOrder
}, pageDots)

local dots = {}
for i = 1, #Scripts do
    local dot = new("Frame", {
        Size = UDim2.fromOffset(6, 6),
        BackgroundColor3 = Color3.fromRGB(255, 255, 255),
        BackgroundTransparency = 0.6,
        LayoutOrder = i
    }, pageDots)
    corner(dot, 3)
    dots[i] = dot
end

local execZone = new("TextButton", {
    Size = UDim2.new(1, -30, 0, 54),
    Position = UDim2.new(0, 15, 1, -70),
    BackgroundColor3 = Theme.AccentDarkRed,
    Text = "CLICK TO EXECUTE",
    Font = Enum.Font.GothamBlack,
    TextSize = 15,
    TextColor3 = Theme.TextMain,
    AutoButtonColor = false,
    ZIndex = 5
}, screen)
corner(execZone, 27)
stroke(execZone, Theme.AccentRed, 1.5)

execZone.MouseEnter:Connect(function()
    TweenService:Create(execZone, TweenInfo.new(0.15), {
        Size = UDim2.new(1, -26, 0, 56),
        BackgroundColor3 = Theme.AccentRed
    }):Play()
end)
execZone.MouseLeave:Connect(function()
    TweenService:Create(execZone, TweenInfo.new(0.15), {
        Size = UDim2.new(1, -30, 0, 54),
        BackgroundColor3 = Theme.AccentDarkRed
    }):Play()
end)

local homeBar = new("Frame", {
    Size = UDim2.fromOffset(120, 5),
    Position = UDim2.new(0.5, -60, 1, -12),
    BackgroundColor3 = Color3.fromRGB(255, 255, 255),
    BackgroundTransparency = 0.3,
    ZIndex = 6
}, screen)
corner(homeBar, 3)

local prevBtn = new("TextButton", {
    Size = UDim2.fromOffset(36, 36),
    Position = UDim2.new(0, 12, 0.5, -18),
    BackgroundColor3 = Color3.fromRGB(0, 0, 0),
    BackgroundTransparency = 0.35,
    Text = "<",
    Font = Enum.Font.GothamBold,
    TextSize = 16,
    TextColor3 = Color3.fromRGB(255, 255, 255),
    AutoButtonColor = false,
    ZIndex = 7
}, screen)
corner(prevBtn, 18)

local nextBtn = new("TextButton", {
    Size = UDim2.fromOffset(36, 36),
    Position = UDim2.new(1, -48, 0.5, -18),
    BackgroundColor3 = Color3.fromRGB(0, 0, 0),
    BackgroundTransparency = 0.35,
    Text = ">",
    Font = Enum.Font.GothamBold,
    TextSize = 16,
    TextColor3 = Color3.fromRGB(255, 255, 255),
    AutoButtonColor = false,
    ZIndex = 7
}, screen)
corner(nextBtn, 18)

local currentIndex = 1
local switching = false
local entryButtons = {}

local function updateListHighlight()
    for idx, btn in pairs(entryButtons) do
        btn.BackgroundColor3 = (idx == currentIndex) and Theme.AccentDarkRed or Theme.ButtonBg
    end
end

local function renderScript(i, direction)
    local data = Scripts[i]

    for d, dot in ipairs(dots) do
        dot.BackgroundTransparency = (d == i) and 0 or 0.6
        dot.Size = (d == i) and UDim2.fromOffset(16, 6) or UDim2.fromOffset(6, 6)
    end

    if switching then return end
    switching = true

    local fadeOut = TweenService:Create(mapImage, TweenInfo.new(0.15, Enum.EasingStyle.Quad), {
        ImageTransparency = 1
    })

    TweenService:Create(titleLabel, TweenInfo.new(0.15), { TextTransparency = 1 }):Play()
    TweenService:Create(descLabel, TweenInfo.new(0.15), { TextTransparency = 1 }):Play()
    fadeOut:Play()

    fadeOut.Completed:Connect(function()
        mapImage.Image = data.Image or ""
        titleLabel.Text = data.Name
        descLabel.Text = data.Desc or ""

        local fadeIn = TweenService:Create(mapImage, TweenInfo.new(0.18, Enum.EasingStyle.Quad), {
            ImageTransparency = 0
        })
        TweenService:Create(titleLabel, TweenInfo.new(0.18), { TextTransparency = 0 }):Play()
        TweenService:Create(descLabel, TweenInfo.new(0.18), { TextTransparency = 0 }):Play()
        fadeIn:Play()
        fadeIn.Completed:Connect(function()
            switching = false
        end)
    end)
end

prevBtn.MouseButton1Click:Connect(function()
    if switching then return end
    currentIndex = currentIndex - 1
    if currentIndex < 1 then currentIndex = #Scripts end
    renderScript(currentIndex, "prev")
    updateListHighlight()
end)

nextBtn.MouseButton1Click:Connect(function()
    if switching then return end
    currentIndex = currentIndex + 1
    if currentIndex > #Scripts then currentIndex = 1 end
    renderScript(currentIndex, "next")
    updateListHighlight()
end)

execZone.MouseButton1Click:Connect(function()
    local data = Scripts[currentIndex]
    print("BatmanScript Loaded -> " .. data.Name)
    launcher:Destroy()
    RunScript(data)
end)

do
    local data = Scripts[currentIndex]
    mapImage.Image = data.Image or ""
    titleLabel.Text = data.Name
    descLabel.Text = data.Desc or ""
    for d, dot in ipairs(dots) do
        dot.BackgroundTransparency = (d == currentIndex) and 0 or 0.6
        dot.Size = (d == currentIndex) and UDim2.fromOffset(16, 6) or UDim2.fromOffset(6, 6)
    end
end

-- ===== SUPPORTED GAMES PANEL =====
local changelog = new("Frame", {
    Name = "ChangelogPanel",
    Size = UDim2.new(0, 220, 0, 580),
    BackgroundColor3 = Theme.PanelBg,
    LayoutOrder = 3
}, container)

corner(changelog, 10)
stroke(changelog, Theme.AccentDarkRed, 1)

new("TextLabel", {
    Size = UDim2.new(1, -20, 0, 40),
    Position = UDim2.fromOffset(15, 5),
    BackgroundTransparency = 1,
    Text = "Supported Games",
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
    ScrollBarImageColor3 = Theme.AccentDarkRed,
    CanvasSize = UDim2.new(0, 0, 0, 0)
}, changelog)

local logLayout = new("UIListLayout", {
    SortOrder = Enum.SortOrder.LayoutOrder,
    Padding = UDim.new(0, 6)
}, scroll)

for index, ScriptData in ipairs(Scripts) do
    if ScriptData.Name ~= "Cortis" then
        local entryBtn = new("TextButton", {
            Name = "LogEntry_" .. index,
            Size = UDim2.new(1, -10, 0, 26),
            BackgroundColor3 = (index == currentIndex) and Theme.AccentDarkRed or Theme.ButtonBg,
            Text = "• " .. ScriptData.Name,
            Font = Enum.Font.GothamMedium,
            TextSize = 11,
            TextColor3 = Theme.TextMain,
            TextXAlignment = Enum.TextXAlignment.Left,
            AutoButtonColor = false,
            LayoutOrder = index
        }, scroll)
        corner(entryBtn, 6)
        entryButtons[index] = entryBtn

        entryBtn.MouseButton1Click:Connect(function()
            if switching or index == currentIndex then return end
            local direction = (index > currentIndex) and "next" or "prev"
            currentIndex = index
            renderScript(currentIndex, direction)
            updateListHighlight()
        end)
    end
end

logLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    scroll.CanvasSize = UDim2.fromOffset(0, logLayout.AbsoluteContentSize.Y + 10)
end)
