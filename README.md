# nvim-silicon

Plugin to create code images using the external `silicon` tool.

This differs from `silicon.nvim` as that plugin uses a rust binding to call directly into the silicon rust library.

## Features

Right now, the plugin supports most options, that the original `silicon` tool offers. The advanced and nice features that @krivahtoo implemented, like window title and watermarking are missing. Clipboard support, might not work cross platform, e.g. inside a WSL2 installation, because from there you do not have access to the system clipboard and there may not be an X server running.

This implementation supports selected line ranges, also highlighting of a line and removing superfluous indents.

Example code image:

![Example code image](https://raw.githubusercontent.com/michaelrommel/nvim-silicon/main/assets/2023-05-04T15-15-04_code.png)

### Ranges

If a range is visually selected it does not matter, whether it is block, line or normally selected. The range is then taken as complete lines: from the line in which the selection starts up to the line in which the selection ends.
If no selection is made, the whole file is taken as input. If you only want to select a single line, then you would have to manually select it with `Shift-V`.

### Highlighting

You can mark a single line as to be highlighted using the standard vim `mark` command with the mark `h`, the default key combination would be `mh`.

## Setup

With the `lazy.nvim` package manager:

```lua
{
	"michaelrommel/nvim-silicon",
	lazy = true,
	cmd = "Silicon",
	config = function()
		require("silicon").setup({
			-- Configuration here, or leave empty to use defaults
			font = "VictorMono NF=34;Noto Emoji=34"
		})
	end
},
```

The `setup` function accepts the following table:

```lua
{
	-- the font settings with size and fallback font
	font = "VictorMono Nerd Font=34;Noto Emoji",
	-- the theme to use, depends on themes available to silicon
	theme = "gruvbox-dark",
	-- the background color outside the rendered os window
	background = "#076678",
	-- a path to a background image
	background_image = nil,
	-- the paddings to either side
	pad_horiz = 100,
	pad_vert = 80,
	-- whether to have the os window rendered with rounded corners
	no_round_corner = false,
	-- whether to put the close, minimize, maximise traffic light controls on the border
	no_window_controls = false,
	-- whether to turn off the line numbers
	no_line_number = false,
	-- with which number the line numbering shall start
	line_offset = 1,
	-- the distance between lines of code
	line_pad = 0,
	-- the rendering of tab characters as so many space characters
	tab_width = 4,
	-- with which language the syntax highlighting shall be done, should be a function
	-- that returns either a language name or an extension like ".js"
	language = function()
		return vim.bo.filetype
	end,
	-- if the shadow below the os window should have be blurred
	shadow_blur_radius = 16,
	-- the offset of the shadow in x and y directions
	shadow_offset_x = 8,
	shadow_offset_y = 8,
	-- the color of the shadow
	shadow_color = "#100808",
	-- whether to strip of superfluous leading whitespace
	gobble = true,
	-- a string or function that defines the path to the output image
	output = function()
		return "./" .. os.date("!%Y-%m-%dT%H-%M-%S") .. "_code.png"
	end,
	-- whether to put the image onto the clipboard, may produce an error if run on WSL2
	to_clipboard = false,
	-- the silicon command, put an absolute location here, if the command is not in your PATH
	command = "silicon",
}
```
