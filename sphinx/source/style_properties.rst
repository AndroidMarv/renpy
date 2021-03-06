.. _style-properties:

================
Style Properties
================

The style properties associated with :ref:`styles <styles>` control how
Displayables are shown. Style properties can be either given without a prefix,
in which case they apply to all states of the displayable, or with a prefix
that limits the property to a limited number of states.


Style Property Prefixes
=======================

Applying a prefix to a style property indicates allows a displayable to change
it's look in response to its focus or selection status. For example, a button
can change its color when the mouse hovers above it, or to indicate when the
choice represented by the button is the currently selected one.

There are five states a displayable can be it.

:dfn:`insensitive`
    Used when the user cannot interact with the displayable.

:dfn:`idle`
    Used when the displayable is neither focused nor selected.

:dfn:`hover`
    Used when the displayable is focused, but not selected.

:dfn:`selected_idle`
    Used when the displayable is not focused, and is selected.

:dfn:`selected_hover`
    Used when the displayable is focused and selected.

Button and Bar displayables (and their variants) update their state, and the
state of their children, in response to events. For example, when the user
puts his mouse over an unselected button, it and all its children will be put
into the hover state.

Style property prefixes allow one to set style properties for the different
states. There is a system of implications set up, so that a prefix can imply
setting the property for more than one state.

The implications are:

.. list-table::
 :header-rows: 1

 * - prefix
   - states implied by prefix

 * - (no prefix)
   - insensitive, idle, hover, selected_idle, selected_hover

 * - ``insensitive_``
   - insensitive

 * - ``idle_``
   - idle, selected_idle

 * - ``hover_``
   - hover, selected_hover

 * - ``selected_``
   - selected_idle, selected_hover

 * - ``selected_idle_``
   - selected_idle

 * - ``selected_hover_``
   - selected_hover

Using a text button, we can show this in action. Text buttons use two styles
by default: ``button`` for the button itself, and ``button_text`` for the
text inside the button. The :propref:`background` style property sets the
background of a button, while the :propref:`color` property sets the color of
text.::

    init python:

     # The button background is gray when insensitive, light
     # blue when hovered, and dark blue otherwise.
     style button:
         background "#006"
         insensitive_background "#444"
         hover_background "#00a"

     # The button text is yellow when selected, and white
     # otherwise.
     style button_text:
         color = "#fff"
         selected_color = "#ff0"

Style Property Values
=====================

Each style property expects a specific kind of data. Many of these are
standard python types, but a few are novel. Here are descriptions of the
novel kinds of value a style property can expect.

`position`
    Positions are used to specify locations relative to the upper-left
    corner of the containing area. (For positions, the containing area is
    given by the layout the displayable is in, if one is given, or the screen
    otherwise. For anchors, the containing area is the size of the
    displayable itself.)

    The way a position value is interpreted depends on the type of the
    value:

    int (like 0, 1, 37, or 42)
        An integer is intepreted as the number of pixels from the left
        or top side of the containing area.
    float (like 0.0, 0.5, or 1.0)
        A floating-point number is intepreted as a fraction of the
        containing area. For example, 0.5 is a point halfway between the
        sides of the containing area, while 1.0 is on the right or bottom
        side.
    renpy.absolute (like renpy.absolute(100.25))
        A renpy.absolute number is intepreted as the number of pixels
        from the left or top side of the screen, when using subpixel-precise
        rendering.

`displayable`
    Any displayable.

`color`
    Colors in Ren'Py can be expressed as strings beginning with the hash
    mark (#), followed by a hex triple or hex quadruple, with each of the
    three or four elements consisting of a one or two hexidecimal character
    color code.

    In a triple, the components represent red, green, and blue. In a
    quadruple, the components represent red, green, blue, and alpha. For
    example:

    * ``"#f00"`` and ``"#ff0000"`` represent an opaque red color.
    * ``"#0f08"`` and ``#00ff0080"`` represent a semi-transparent green
      color.

    The color triples are the same as used in HTML.

    Colors can also be represented as a 4-component tuple, with the 4
    components being integers between 0 and 255. The components correspond to
    red, green, blue, and alpha, in that order.

    * ``(0, 0, 255, 255)`` represents an opaque blue color.

List of All Style Properties
============================

The style properties control the look of the various displayables. Not all
style properties apply to all displayables, so we've divided them up into
groups.

.. _position-style-properties:

Position Style Properties
-------------------------

These are used to control the position of a displayable inside the area
allocated to it by a layout, or on the screen when not inside a layout.

.. style-property:: xpos position

    The position of the displayable relative to the left side of the
    containing area.

.. style-property:: ypos position

    The position of the displayable relative to the right side of the
    containing area.

.. style-property:: pos tuple of (position, position)

    Equivalent to setting xpos to the first component of the tuple,
    and ypos to the second component of the tuple.

.. style-property:: xanchor position

    The position of the anchor relative to the left side of the
    displayable.

.. style-property:: yanchor position

    The position of the anchor relative to the top side of the
    displayable.

.. style-property:: anchor tuple of (position, position)

    Equivalent to setting xanchor to the first component of the tuple,
    and yanchor to the second component of the tuple.

.. style-property:: xalign float

    Equivalent to setting xpos and xanchor to the same value. This has
    the effect of placing the displayable at a relative location on
    the screen, with 0.0 being the left side, 0.5 the center, and 1.0
    being the right side.

.. style-property:: yalign float

    Equivalent to setting ypos and yanchor to the same value. This has
    the effect of placing the displayable at a relative location on
    the screen, with 0.0 being the top, 0.5 the center, and 1.0
    the bottom.

.. style-property:: align tuple of (float, float)

    Equivalent to setting xalign to the first component of the tuple,
    and yalign to the second.

.. style-property:: xcenter position

    Equivalent to setting xpos to the value of this property, and
    xanchor to 0.5.

.. style-property:: ycenter position

    Equivalent to setting ypos to the value of tihis property, and
    yanchor to 0.5.

.. style-property:: xoffset int

    Gives a number of pixels that are added to the horizontal position
    computed using xpos and xalign.

.. style-property:: yoffset int

    Gives a number of pixels that are added to the vertical position
    computed using ypos and yalign.

.. style-property:: xmaximum int

    Specifies the maximum horizontal size of the displayable, in pixels.

.. style-property:: ymaximum int

    Specifies the maximum vertical size of the displayable in pixels.

.. style-property:: maximum tuple of (int, int)

    Equivalent to setting xmaximum to the first component of the
    tuple, and ymaximum to the second.

.. style-property:: xminimum int

    Sets the minimum width of the displayable, in pixels. Only works
    with displayables that can vary their size.

.. style-property:: yminimum int

    Sets the minimum height of the displayables, in pixels. Only works
    with displayables that can vary their size.

.. style-property:: minimum tuple of (int, int)

    Equivalent to setting xminimum to the first component of the
    tuple, and yminimum to the second.

.. style-property:: xsize int

    Equivalent to setting xminimum and xmaximum to the same value. This
    has the effect of setting the width of the displayable.

.. style-property:: ysize int

    Equivalent to setting yminimum and ymaximum to the same value. This
    has the effect of setting the height of the displayable.

.. style-property:: xysize tuple of (int, int)

    Equivalent to setting xminimum and xmaximum to the first component of
    the tuple, and yminimum and ymaximum to the second component. This
    has the effect of setting the size of the displayable.

.. style-property:: xfill boolean

    If true, the displayable will expand to fill all available
    horizontal space. If not true, it will only be large enough to
    contain its children.

    This only works for displayables that can change size.

.. style-property:: yfill boolean

    If true, the displayable will expand to fill all available
    vertical space. If not true, it will only be large enough to
    contain its children.

    This only works for displayables that can change size.

.. style-property:: area tuple of (int, int, int, int)

    The tuple is interpreted as (`xpos`, `ypos`, `width`,
    `height`). Attempts to position the displayable such that it's
    upper-left corner is at `xpos` and `ypos`, and its size is `width`
    and `height`.

    It does this by setting the xpos, ypos, xanchor, yanchor,
    xmaximum, ymaximum, xminimum, yminimum, xfill, and yfill
    properties to appropriate values.

    This will not work with all displayables and all layouts.


.. _text-style-properties:

Text Style Properties
---------------------

.. style-property:: antialias boolean

    If True, the default, truetype font text will be rendered
    anti-aliased.

.. style-property:: black_color color

    When rendering an image-based font, black will be mapped to this
    color. This has no effect for truetype fonts.

.. style-property:: bold boolean

    If True, render the font in a bold style. For a truetype font,
    this usually involves synthetically increasing the font weight. It
    can also cause the font to be remapped, using
    :var:`config.font_replacement_map`.

.. style-property:: caret displayable or None

    If not None, this should be a displayable. The input widget will
    use this as the caret at the end of the text. If None, a 1 pixel
    wide line is used as the caret.

.. style-property:: color color

    The color the text is rendered in. When using a truetype font,
    the font is rendered in this color. When using an image-based
    font, white is mapped to this color.

.. style-property:: first_indent int

    The amount that the first line of text in a paragraph is indented
    by, in pixels.

.. style-property:: font string

    A string giving the name of the font used to render text.

    For a truetype font file, this is usually the name of the file
    containing the font (like ``"DejaVuSans.ttf"``). To select a second
    font in a collection, this can be prefixed with a number and
    at sign (like ``"0@font.ttc"`` or ``"1@font.ttc"``). For an
    image-based font, this should be the name used to register the
    font.

.. style-property:: size int

    The size of the font on the screen. While this is nominally in
    pixels, font files may have creative interpretations of this
    value.

.. style-property:: italic boolean

    If true, the text will be rendered in italics. For a truetype font,
    this usually involves synthetically increasing the font slant. It
    can also cause the font to be remapped, using
    :var:`config.font_replacement_map`.

.. style-property:: justify boolean

    If true, additional whitespace is inserted between words so that
    the left and right margins of each line are even. This is not
    performed on the last line of a paragraph.

.. style-property:: kerning float

    A kerning adjustment, the number of pixels of space that's added
    between each pair of characters. (This can be negative to remove
    space between characters.)

.. style-property:: language string

    Controls the language family used to break text into lines. Legal
    values are:

    ``"unicode"`` (default)
        Uses the unicode linebreaking algorithm, which is suitable for
        most languages.

    ``"japanese-strict"``
        Formats Japanese text in a "strict" manner. It
        forbids breaks before small kana and prolonged sound marks.

    ``"japanese-normal"``
        Formats Japanese text in a "normal" manner. It
        allows breaks before small kana, prolonged sound marks, and
        certain hyphens.

    ``"japanese-loose"``
        Formats Japanese text in a "loose" manner. It allows breaks
        before small kana , prolonged sound marks, iteration marks,
        inseparable characters, centered punctuation marks, and postfixes;
        and allows breaks before prefixes.

    ``"korean-with-spaces"``
        Used for Korean text delimited by whitespace. This prevents linebreaking
        between adjacent Korean characters.

    ``"western"``
        Allows breaking only at whitespace. Suitable for most
        languages.

    The three Japanese breaking modes are taken from the `CSS3 text module <http://www.w3.org/TR/css3-text/#line-break>`_.

.. style-property:: layout string

    Controls how words are allocated to lines. Legal values are:

    ``"tex"`` (default)
        Uses the Knuth-Plass linebreaking algorithm, which attempts to minimize
        the difference in line lengths of all but the last line.

    ``"subtitle"``
        Uses the Knuth-Plass linebreaking algorithm, but attempts to even out
        the lengths of all lines.

    ``"greedy"``
        A word is placed on the first line that has room for it.

    ``"nowrap"``
        Do not line-break.

.. style-property:: line_leading int

    The number of pixels of spacing to include above each line.

.. style-property:: line_overlap_split int

    When in slow text mode, and two lines overlap, this many pixels of
    the overlap are allocated to the top line. Increase this if the
    bottoms of characters on the top line are clipped.

.. style-property:: line_spacing int

    The number of pixels of spacing to include below each line.

.. style-property:: min_width int

    Sets the minimum width of each line of that. If a line is shorter
    than this, it is padded to this length, with text_align used to
    specify where such padding is placed.

.. style-property:: newline_indent boolean

    If true, the :propref:`first_indent` indentation is used after
    each newline in a string. Otherwise, the :propref:`rest_indent`
    indentation is used.

.. style-property:: outlines list of tuple of (int, color, int, int)

    This is a list of outlines that are drawn behind the text. Each
    tuple specifies an outline, and outlines are drawn from back to
    front.

    The list contains (`size`, `color`, `xoffset`, `yoffset`)
    tuples. `Size` is the amount the font is expanded by, in
    pixels. `Color` is the color of the outline. `xoffset` and
    `yoffset` are the amount the outline is shifted by, in pixels.

    The outline functionality can also be used to give drop-shadows to
    fonts, by specifiying a size of 0 and non-zero offsets.

    Outlines only work with truetype fonts.

.. style-property:: rest_indent int

    Specifies the number of pixels the second and later lines in a
    paragraph are indented by.

.. style-property:: ruby_style style or None

    If not None, this should be a style object. The style that's used for
    ruby text.

.. style-property:: slow_cps int or True

    If a number, shows text at the rate of that many characters per
    second. If True, shows text at the speed taken from the "Text
    Speed" preference.

.. style-property:: slow_cps_multiplier float

    The speed of the text is multiplied by this number. This can be
    used to have a character that speeks at a faster-than-normal rate
    of speed.

.. style-property:: strikethrough boolean

    If True, a line is drawn through the text.

.. style-property:: text_align float

    This is used when a line is shorter than the width of the text
    displayable. It determines how much of the extra space is placed
    on the left side of the text. (And hence, the text alignment.)

    0.0 will yield left-aligned text, 0.5 centered text, and 1.0
    right-aligned text.

.. style-property:: underline boolean

    If true, an underline will be added to the text.

.. style-property:: hyperlink_functions tuple of (function, function, function)

    This is a tuple of three functions relating to hyperlinks in text.

    The first item is the hyperlink style function. When called with a single
    argument, the argument of the hyperlink, it must return a style object to
    use for the hyperlink, such as ``style.hyperlink_text``. Note that a
    style object is not a string.

    The second item is the hyperlink clicked function. This function is called
    when a hyperlink is chosen by the user. If it returns a value other than
    None, the interaction returns that value.

    The third item is the hyperlink focus function. This function is called
    with the argument of the hyperlink when the hyperlink gains focus, and
    with None when it loses focus. If it returns a value other than None,
    the interaction returns that value

.. style-property:: vertical boolean

    If true, the text will be rendered vertically.

.. _window-style-properties:

Window Style Properties
-----------------------

Window properties are used to specify the look of windows, frames, and buttons.

.. style-property:: background displayable or None

    A displayable that is used as the background of the window. This
    is often a :func:`Frame`, allowing the size of the background to
    scale with the size of the window.

    If None, no background is drawn, but other properties function as
    if the background was present.

.. style-property:: foreground displayable or None

    If not None, this displayable is drawn above the contents of the
    window.

.. style-property:: left_margin int

    The amount of transparent space to the left of the background, in
    pixels.

.. style-property:: right_margin int

    The amount of transparent space to the right of the background, in
    pixels.

.. style-property:: xmargin int

    Equivalent to setting left_margin and right_margin to the same
    value.

.. style-property:: top_margin int

    The amount of transparent space above the background, in pixels.

.. style-property:: bottom_margin int

    The amount of transparent space below the background, in pixels.

.. style-property:: ymargin int

    Equivalent to setting top_margin and bottom_margin to the same
    value.

.. style-property:: left_padding int

    The amount of space between the background and the left side of
    the window content, in pixels.

.. style-property:: right_padding int

    The amount of space between the background and the right side of
    the window content, in pixels.

.. style-property:: xpadding int

    Equivalent to setting left_padding and right_padding to the same
    value.

.. style-property:: top_padding int

    The amount of space between the background and the top side of
    the window content, in pixels.

.. style-property:: bottom_padding int

    The amount of space between the background and the bottom side of
    the window content, in pixels.

.. style-property:: ypadding int

    Equivalent to setting top_padding and bottom_padding to the same
    value.

.. style-property:: size_group string or None

    If not None, this should be a string. Ren'Py will render all
    windows with the same size_group value at the same size.


.. _button-style-properties:

Button Style Properties
-----------------------

.. style-property:: child displayable or None

    If not None, this gives a displayable that replaces the child of the
    button. For example, this (as insensitive_child) can be used to replace the
    contents of an insensitive button with an image that indicates the button
    is locked.

.. style-property:: hover_sound string

    A sound that is played when the button gains focus.

.. style-property:: activate_sound string

    A sound that is played when the button is clicked.

.. style-property:: mouse string

    The mouse style that is used when the button is focused. This
    should be one of the styles in :var:`config.mouse`.

.. style-property:: focus_mask multiple

    A mask that's used to control what portions of the button can be
    focused, and hence clicked on. The type of this propertie determines
    how it is interpreted.

    Displayable
        The areas of the displayable that are not transparent cause the button
        to be focused.
    True
        The button itself is used as the displayable (so
        non-transparent areas of the button cause the button to be
        focused).
    callable
        If a non-displayable callable (like a function, method, or object
        with a __call__ method) is given, the function is called with two
        arguments, the x and y offset from the top-left corner of the
        displayable. If the function returns true, the displayable is
        focused.
    None
        If none is given, the entire button can be focused.


.. _bar-style-properties:

Bar Style Properties
--------------------

Bars are drawn with gutters on the left and right, that when clicked can cause
the bar to move by a small amount. The remaining space is the portion of the
bar that can change, with the amount on each side proportional to the bar's
value as a fraction of the range.

The thumb is an area in the center of the bar that can be dragged by the user.

When a bar is drawn, the thumb's shadow is drawn first. Then the left/bottom
and right/top sides of the bar, followed by the thumb itself.

Note that the valid sides of a bar depend on the value of the bar_vertical
property. If it's True, the top and bottom sides are relevant. Otherwise, the
left and right sides are used.

.. style-property:: bar_vertical boolean

    If true, the bar has a vertical orientation. If false, it has a
    horizontal orientation.

.. style-property:: bar_invert boolean

    If true, the value of the bar is represented on the right/top
    side of the bar, rather than the left/bottom side.

.. style-property:: bar_resizing boolean

    If true, we resize the sides of the bar. If false, we render the
    sides of the bar at full size, and then crop them.

.. style-property:: left_gutter int

    The size of the gutter on the left side of the bar, in pixels.

.. style-property:: right_gutter int

    The size of the gutter on the right side of the bar, in pixels.

.. style-property:: top_gutter int

    The size of the gutter on the top side of the bar, in pixels.

.. style-property:: bottom_gutter int

    The size of the gutter on the bottom side of the bar, in pixels.

.. style-property:: left_bar displayable

    The displayable used for the left side of the bar.

.. style-property:: right_bar displayable

    The displayable used for the right side of the bar.

.. style-property:: top_bar displayable

    The displayable used for the top side of the bar.

.. style-property:: bottom_bar displayable

    The displayable uses for the bottom side of the bar.

.. style-property:: thumb displayable or None

    If not None, this is a displayable that is drawn over the break
    between the sides of the bar.

.. style-property:: thumb_shadow displayable or None

    If not None, this is a displayable that is drawn over the break
    between the sides of the bar.

.. style-property:: thumb_offset int

    The amount that by which the thumb overlaps the bars, in
    pixels. To have the left and right bars continue unbroken, set
    this to half the width of the thumb in pixels.

.. style-property:: mouse string

    The mouse style that is used when the button is focused. This
    should be one of the styles in :var:`config.mouse`.

.. style-property:: unscrollable string or None

    Controls what happens if the bar is unscrollable (if the range is
    set to 0, as is the case with a viewport containing a displayable
    smaller than itself). There are three possible values:

    ``None``
        Renders the bar normally.

    ``"insensitive"``
        Renders the bar in the insensitive state. This allows the
        bar to change its style to reflect its lack of usefulness.

    ``"hide"``
       Prevents the bar from rendering at all. Space will be allocated
       for the bar, but nothing will be drawn in that space.


.. _box-style-properties:

Box Style Properties
--------------------

These are used for the horizontal and vertical box layouts.

.. style-property:: spacing int

    The spacing between members of this box, in pixels.

.. style-property:: first_spacing int

    If not None, the spacing between the first and second members of
    this box, in pixels. This overrides the spacing property.

.. style-property:: box_reverse boolean

    If true, the placement of the items in the box will be reversed. When
    this is true, a hbox will be filled right-to-left, and a vbox will
    be filled bottom-to-top. This defaults to false.

.. style-property:: box_wrap boolean

    If true, then boxes will wrap when they reach the end of a line or column.
    If false (the default), they will extend past the end of the line.

.. style-property:: order_reverse boolean

    If false, the default, the items in the box will be draw first-to-last,
    with the first item in the box being below the second, and so on. If true,
    this order will be reversed, and the first item in the box will be above
    all other items in the box.


.. _fixed-style-properties:

Fixed Style Properties
----------------------

These are used with the fixed layout.

.. style-property:: fit_first bool

   If true, then the size of the fixed layout is shrunk to be equal with
   the size of the first item in the layout.
