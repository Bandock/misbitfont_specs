# MisbitFont V0.1
First Author:  Joshua Moss (Bandock)


## Specification
Palette Format Support:  1-bit to 8-bit supported
Encoding Support:  ASCII/ANSI, UTF-8/UTF-16/UTF-32, and possibly many others


## Format
```
Header
{
	4 Bytes = MSBT (MSBT in Little Endian Form)
	{
		Description:  Magic word to determine if it's a MisbitFont file.  Read correctly if on a system that utilizes the little endian format.
	}
	4 Bytes = Version (Little Endian Form)
	{
		Description:  Used to determine what version of MisbitFont the file was made with.  Very useful for performing feature checks.  This is the little endian form of it.

		2 Bytes = Major (Little Endian Form, Currently 0)
		2 Bytes = Minor (Little Endian Form, Currently 1)
	}
	4 Bytes = TBSM (MSBT in Big Endian Form)
	{
		Description:  Magic word to determine if it's a MisbitFont file.  Read correctly if on a system that utilizes the big endian format.
	}
	4 Bytes = Version (Big Endian Form)
	{
		Description:  Used to determine what version of MisbitFont the file was made with.  Very useful for performing feature checks.  This is the big endian form of it.

		2 Bytes = Major (Big Endian Form, Currently 0)
		2 Bytes = Minor (Big Endian Form, Currently 1)
	}
	1 Byte = Palette Format
	{
		Description:  Used to specify how many bits should be used per pixel for color processing on the application's end.

		0 = 1-bit
		1 = 2-bit
		2 = 3-bit
		3 = 4-bit
		4 = 5-bit
		5 = 6-bit
		6 = 7-bit
		7 = 8-bit
		8 to 255 = Currently unused
	}
	1 Byte = Max Font Width (0 = 1px, 255 = 256px)
	1 Byte = Max Font Height (0 = 1px, 255 = 256px)
	1 Byte = Flags
	{
		0x01 = Spacing Type
		{
			Description:  Used to determine if the entire font set is monospaced or support variable spacing on various characters.  When variable spacing is enabled, a table is provided to determine the width of each character.

			Off = Monospace (Fixed)
			On = Variable
		}
	}
	4 Bytes = Font Character Count (Little Endian Form)
	{
		Description:  Used to determine how many characters are stored in a MisbitFont file.  Also used to determine the size of the variable size table if Variable Spacing Type is used.  This is the little endian form.
	}
	4 Bytes = Font Character Count (Big Endian Form)
	{
		Description:  Used to determine how many characters are stored in a MisbitFont file.  Also used to determine the size of the variable size table if Variable Spacing Type is used.  This is the big endian form.
	}
	64 Bytes = Font Name (Uses UTF-8 Text Encoding)
	{
		Description:  Useful for giving names to MisbitFont files, which describes what kind of fonts they are.  This data can be parsed by the application (UTF-8 Text Encoding must be supported to parse this field).
	}
	64 Bytes = Language (Uses UTF-8 Text Encoding)
	{
		Description:  This field is very useful for localization as it can aid in loading up fonts for foreign languages.  How this data is parsed is up to the application (UTF-8 Text Encoding must be supported to parse this field).
	}
}
```

Variable Size Type Table (Size determined by Font Character Count)
{
	Description:  Used to specify the font width of each character to aid in variable spacing solutions (which is handled by the application).  This table only exists if Variable Spacing Type is enabled.

	1 Byte Per Entry = Character Width (0 = 1px, 255 = 256px, must not be larger than Max Font Width)
}

Font Data (Size determined by Palette Format, Max Font Width, Max Font Height, and Font Character Count)
{
	Description:  This is where all the font data is stored.  Each font character is stored individually one by one based on the Palette Format, Max Font Width, and Max Font Height.  Processing starts at the most significant bit of the first byte, which is the upper left corner of the font character.  Font Character Count further determines how many characters are stored in this area.
}
