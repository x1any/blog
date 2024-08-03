---
title: '跨平台终端模拟器 Wezterm'
date: 2024-03-02T00:00:00+08:00
lastmod:
draft: false
authors: [ethan]
description: WezTerm 是一个强大的跨平台终端模拟器，由 @wez 编写，使用 Rust 实现
tags: [wezterm]
categories: [笔记]
series:
series_weight:
seriesNavigation: true
featuredImage: https://img.hsien.top/3KmVPSgVfqW63tHU4bWF.png?imageMogr2/format/webp
featuredImagePreview: https://img.hsien.top/3KmVPSgVfqW63tHU4bWF.png?imageMogr2/format/webp
hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: true
lightgallery: false # 如果设为 true, 文章中的图片将可以按照画廊形式呈现
---
<!--more-->

## 官网

* [WezTerm - Wez&apos;s Terminal Emulator](https://wezfurlong.org/wezterm/index.html "WezTerm - Wez's Terminal Emulator")
* [Releases · wez/wezterm · GitHub](https://github.com/wez/wezterm/releases/ "Releases · wez/wezterm · GitHub")

## 配置

路径 `~/.config/wezterm/wezterm.lua`

```lua
local wezterm = require 'wezterm';

local default_prog = { 'ucrt64.exe', '-defterm', '-here', '-no-start', '-shell', 'zsh' }

local launch_menu = {}
if wezterm.target_triple == 'x86_64-pc-windows-msvc' then
    table.insert(launch_menu, {
        label = 'Windows PowerShell',
        args = { 'powershell.exe', '-NoLogo' },
    })

    table.insert(launch_menu, {
        label = 'CMD',
        args = { 'cmd.exe', '-NoLogo' },
    })
    
    table.insert(launch_menu,  {
        label = 'UCRT64 / zsh', 
        args = { 'ucrt64.exe', '-defterm', '-here', '-no-start', '-shell', 'zsh' }
    })

    -- Find installed visual studio version(s) and add their compilation
    -- environment command prompts to the menu
    for _, vsvers in
    ipairs(
        wezterm.glob('Microsoft Visual Studio/20*', 'C:/Program Files (x86)')
    )
    do
        local year = vsvers:gsub('Microsoft Visual Studio/', '')
        table.insert(launch_menu, {
            label = 'x64 Native Tools VS ' .. year,
            args = {
                'cmd.exe',
                '/k',
                'C:/Program Files (x86)/'
                .. vsvers
                .. '/BuildTools/VC/Auxiliary/Build/vcvars64.bat',
            },
        })
    end
end

local materia = wezterm.color.get_builtin_schemes()['Material Darker (base16)']
materia.scrollbar_thumb = '#CFCFCF'

local act = wezterm.action;
local key_bindings = {
    -- F11 切换全屏
    { key = 'F11',       mods = 'NONE',       action = act.ToggleFullScreen },
    -- 显示启动菜单
    { key = 'l',         mods = 'ALT',        action = act.ShowLauncher },
    -- Ctrl+Shift+N 新窗口
    { key = 'N',         mods = 'SHIFT|CTRL', action = act.SpawnWindow },
    -- Ctrl+Shift+Tab 遍历 tab
    { key = "Tab",       mods = "SHIFT|CTRL", action = act.ActivateTabRelative(1) },
    -- Ctrl+Shift+W 关闭 panel 且不进行确认
    { key = 'W',         mods = 'SHIFT|CTRL', action = act.CloseCurrentPane { confirm = false } },
    -- Ctrl+Shift+PageUp 向上滚动一页
    { key = 'PageUp',    mods = 'SHIFT|CTRL', action = act.ScrollByPage(-1) },
    -- Ctrl+Shift+PageDown 向下滚动一页
    { key = 'PageDown',  mods = 'SHIFT|CTRL', action = act.ScrollByPage(1) },
    -- Ctrl+Shift+UpArrow 向上滚动一行
    { key = 'UpArrow',   mods = 'SHIFT|CTRL', action = act.ScrollByLine(-1) },
    -- Ctrl+Shift+DownArrow 向下滚动一行
    { key = 'DownArrow', mods = 'SHIFT|CTRL', action = act.ScrollByLine(1) },
}

local mouse_bindings = {
    -- Click 不打开链接
    {
        event = { Up = { streak = 1, button = "Left" } },
        mods = "NONE",
        action = act.CompleteSelection("PrimarySelection"),
    },
    -- CTRL-Click 打开链接
    {
        event = { Up = { streak = 1, button = "Left" } },
        mods = "CTRL",
        action = act.OpenLinkAtMouseCursor,
    },
    -- RCLick 粘贴
    {
        event = { Down = { streak = 1, button = "Right" } },
        mods = "NONE",
        action = act.PasteFrom("Clipboard")
    },
    -- CTRL-RCLick 复制
    {
        event = { Down = { streak = 1, button = "Right" } },
        mods = "CTRL",
        action = act.CopyTo("Clipboard")
    },
}

local config = {}
if wezterm.config_builder then
    config = wezterm.config_builder()
end
config = {
    exit_behavior = "Close",
    initial_cols = 120,
    initial_rows = 36,
    font_size = 12,
    tab_max_width = 32,
    enable_scroll_bar = true,
    enable_tab_bar = true,
    enable_wayland = false,
    colors = materia,
    window_decorations = "INTEGRATED_BUTTONS|RESIZE",
    window_background_opacity = 0.95,
    window_padding = { left = 10, right = 10, top = 10, bottom = 10 },
    pane_focus_follows_mouse = true,
    font = wezterm.font_with_fallback({ "Fira Code Retina", "Noto Sans SC", "MesloLGS NF" }),
    default_prog = default_prog,
    launch_menu = launch_menu,
    keys = key_bindings,
    mouse_bindings = mouse_bindings,
    disable_default_key_bindings = false,
}

return config
```