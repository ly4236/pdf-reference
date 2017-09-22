# PDF Reference

## CHAPTER 1 Introduction

    The Adobe Portable Document Format (PDF) is the native file format of the Adobe® Acrobat®family of products.

    Adobe Portable Document Format (PDF)是Adobe的产品。

    The goal of these products is to enable users to exchange and view electronic documents easily and reliably, independently ofthe environment in which they were created.

    它的目标是为了使用户能在任何环境下查看和交换电子文件。

    PDF relies on the same imaging model as the PostScript® page description language to describe text and graphics in a device-independent and resolution-independent manner.

    类似PostScript的语言来描述的文件，独立于设备和分辨率。

    To improve performance for interactive viewing, PDF defines a more structured format than that used by most PostScript language programs.

    为了提高性能，pdf提供一种比PostScript语言更为结构化的格式。

    PDF also includes objects, such as annotations and hypertext links, that are not part of the page itself but are useful for interactive viewing and document interchange.

    PDF还包含对象，注释和超链接。

### About This Book

    This book provides a description of the PDF file format and is intended primarily for developers of PDF producer applications that create PDF files directly.

    这本书用来描述PDF文件格式，主要用于制作PDF文件生成应用的开发者。

    It also contains enough information to allow developers to write PDF consumer applications that read existing PDF files and interpret or modify their contents.

    它也包含了足够的信息让开发者可以读取现有的PDF文件，然后去解释或者读取它们的内容。

    Although the PDF Reference is independent of any particular software implementation, some PDF features are best explained by describing the way they are processed by a typical application program.

    虽然PDF独立于任何软件特性,但是通过经典的应用程序的方式去处理他们，可能可以更好的解释PDF的一些特性

    In such cases, this book uses the Acrobat family of PDF viewer applications as its model. (The prototypical viewer is the fully capable Acrobat product, not the limited Adobe Reader® product.)

    Appendix C discusses some implementation limits in the Acrobat viewer applications, even though these limits are not part of the file format itself.

    Appendix H provides compatibility and implementation notes that describe how Acrobat viewers behave when they encounter newer features they do not understand and specify areas in which the Acrobat products diverge from the specification presented in this book.

    Implementors of PDF producer and consumer applications can use this information as guidance.

    This edition of the PDF Reference describes version 1.7 of PDF. (See implementation note 1 in Appendix H.)

    Throughout the book, information specific to particular versions of PDF is marked with indicators such as (PDF 1.3) or (PDF 1.4).

    Features so marked may be new or substantially redefined in that version.

    Features designated (PDF 1.0) have generally been superseded in later versions; unless otherwise stated, features identified as specific to other versions are
    understood to be available in later versions as well. (PDF consumer applications designed for a specific PDF version generally ignore newer features they do not recognize; implementation notes in Appendix H point out exceptions.)

    Note: In this edition, the term consumer is generally used to refer to PDF processing applications; viewer is reserved for applications that implement features that interact with users.

    This distinction is not always clear, however, since non-interactive applications may process objects in PDF documents (such as annotations) that represent interactive features.

    The rest of the book is organized as follows:

* Chapter 2 “Overview” 概述

    briefly introduces the overall architecture of PDF and
the design considerations behind it, compares it with the PostScript language,
and describes the underlying imaging model that they share.

* Chapter 3 “Syntax” 语法

    presents the syntax of PDF at the object, file, and document
level. It sets the stage for subsequent chapters, which describe how that
information is interpreted as page descriptions, interactive navigational aids,
and application-level logical structure.

* Chapter 4 “Graphics” 图形

    describes the graphics operators used to describe the appearance of pages in a PDF document.

* Chapter 5 “Text” 文本

    discusses PDF’s special facilities for presenting text in the
form of character shapes, or glyphs, defined by fonts.

* Chapter 6 “Rendering” 渲染

    considers how device-independent content descriptions
are matched to the characteristics of a particular output device.

* Chapter 7 “Transparency” 透明度

    discusses the operation of the transparent imaging
model, introduced in PDF 1.4, in which objects can be painted with varying
degrees of opacity, allowing the previous contents of the page to show through. 

* Chapter 8 “Interactive Features” 互动功能

    describes those features of PDF that allow a
user to interact with a document on the screen by using the mouse and keyboard.

* Chapter 9 “Multimedia Features” 多媒体功能

    describes those features of PDF that support
embedding and playing multimedia content, including video, music and 3D
artwork.

* Chapter 10 “Document Interchange” 文档交换

    shows how PDF documents can incorporate higher-level information that is useful for the interchange of documents among applications.

* Appendix A “Operator Summary,”

    lists all the operators used in describing the visual content of a PDF document.

* Appendix B, “Operators in Type 4 Functions,”

    summarizes the PostScript operators that can be used in PostScript calculator functions, which contain code written in a small subset of the PostScript language.

* Appendix C, “Implementation Limits,”

    describes typical size and quantity limits imposed by the Acrobat viewer applications.

* Appendix D, “Character Sets and Encodings,”

    lists the character sets and encodings that are assumed to be predefined in any PDF consumer application.

* Appendix E, “PDF Name Registry,”

    discusses a registry, maintained for developers by Adobe Systems, that contains private names and formats used by PDF producers or Acrobat plug-in extensions.

* Appendix F, “Linearized PDF,”

    describes a special form of PDF file organization designed to work efficiently in network environments.

* Appendix G, “Example PDF Files,”

    presents several examples showing the structure of actual PDF files, ranging from one containing a minimal one-page document to one showing how the structure of a PDF file evolves over the course of several revisions.

* Appendix H, “Compatibility and Implementation Notes,”

    provides details on the behavior of Acrobat viewer applications and describes how consumer applications should handle PDF files containing features that they do not recognize.

* Appendix I, “Computation of Object Digests,”

    describes in detail an algorithm for calculating an object digest (discussed in Section 8.7, “Digital Signatures”).

A color plate section provides illustrations of some of PDF’s color-related features.References in the text of the form “see Plate 1” refer to the contents of this section.

The book concludes with a Bibliography and an Index.

## CHAPTER 2 Overview 概述

    PDF is a file format for representing documents in a manner independent of the
    application software, hardware, and operating system used to create them and of
    the output device on which they are to be displayed or printed. A PDF document
    consists of a collection of objects that together describe the appearance of one or
    more pages, possibly accompanied by additional interactive elements and higherlevel
    application data. A PDF file contains the objects making up a PDF document
    along with associated structural information, all represented as a single selfcontained
    sequence of bytes.
    A document’s pages (and other visual elements) can contain any combination of
    text, graphics, and images. A page’s appearance is described by a PDF content
    stream, which contains a sequence of graphics objects to be painted on the page.
    This appearance is fully specified; all layout and formatting decisions have already
    been made by the application generating the content stream.
    In addition to describing the static appearance of pages, a PDF document can
    contain interactive elements that are possible only in an electronic representation.
    PDF supports annotations of many kinds for such things as text notes, hypertext
    links, markup, file attachments, sounds, and movies. A document can define its
    own user interface; keyboard and mouse input can trigger actions that are specified
    by PDF objects. The document can contain interactive form fields to be filled
    in by the user, and can export the values of these fields to or import them from
    other applications.
    Finally, a PDF document can contain higher-level information that is useful for
    interchange of content among applications. In addition to specifying appearance,
    a document’s content can include identification and logical structure information that allows it to be searched, edited, or extracted for reuse elsewhere. PDF is particularly
    well suited for representing a document as it moves through successive
    stages of a prepress production workflow.

### Imaging Model 成像模型

    At the heart of PDF is its ability to describe the appearance of sophisticated
    graphics and typography. This ability is achieved through the use of the Adobe
    imaging model, the same high-level, device-independent representation used in
    the PostScript page description language.
    Although application programs could theoretically describe any page as a fullresolution
    pixel array, the resulting file would be bulky, device-dependent, and
    impractical for high-resolution devices. A high-level imaging model enables
    applications to describe the appearance of pages containing text, graphical
    shapes, and sampled images in terms of abstract graphical elements rather than
    directly in terms of device pixels. Such a description is economical and deviceindependent,
    and can be used to produce high-quality output on a broad range of
    printers, displays, and other output devices. 

#### Page Description Languages 页描述语言

    Among its other roles, PDF serves as a page description language, a language for
    describing the graphical appearance of pages with respect to an imaging model.
    An application program produces output through a two-stage process:
    1. The application generates a device-independent description of the desired output
    in the page description language.
    2. A program controlling a specific output device interprets the description and
    renders it on that device.
    The two stages may be executed in different places and at different times. The
    page description language serves as an interchange standard for the compact, device-independent
    transmission and storage of printable or displayable documents.

####  Adobe Imaging Model

    The Adobe imaging model is a simple and unified view of two-dimensional
    graphics borrowed from the graphic arts. In this model, “paint” is placed on a
    page in selected areas:
    • The painted figures can be in the form of character shapes (glyphs), geometric
    shapes, lines, or sampled images such as digital representations of photographs.
    • The paint may be in color or in black, white, or any shade of gray. It may also
    take the form of a repeating pattern (PDF 1.2) or a smooth transition between
    colors (PDF 1.3).
    • Any of these elements may be clipped to appear within other shapes as they are
    placed onto the page.
    A page’s content stream contains operands and operators describing a sequence of
    graphics objects. A PDF consumer application maintains an implicit current page
    that accumulates the marks made by the painting operators. Initially, the current
    page is completely blank. For each graphics object encountered in the content
    stream, the application places marks on the current page, which replace or combine
    with any previous marks they may overlay. Once the page has been completely
    composed, the accumulated marks are rendered on the output medium
    and the current page is cleared to blank again.
    PDF 1.3 and earlier versions use an opaque imaging model in which each new
    graphics object painted onto a page completely obscures the previous contents of
    the page at those locations (subject to the effects of certain optional parameters
    that may modify this behavior; see Section 4.5.6, “Overprint Control”). No matter
    what color an object has—white, black, gray, or color—it is placed on the page
    as if it were applied with opaque paint. PDF 1.4 introduces a transparent imaging
    model in which objects painted on the page are not required to be fully opaque.
    Instead, newly painted objects are composited with the previously existing contents
    of the page, producing results that combine the colors of the object and its
    backdrop according to their respective opacity characteristics. The transparent
    imaging model is described in Chapter 7.
    The principal graphics objects (among others) are as follows:
    • A path object consists of a sequence of connected and disconnected points,
    lines, and curves that together describe shapes and their positions. It is built up through the sequential application of path construction operators, each of which
    appends one or more new elements. The path object is ended by a path-painting
    operator, which paints the path on the page in some way. The principal pathpainting
    operators are S (stroke), which paints a line along the path, and f (fill),
    which paints the interior of the path.
    • A text object consists of one or more glyph shapes representing characters of
    text. The glyph shapes for the characters are described in a separate data structure
    called a font. Like path objects, text objects can be stroked or filled.
    • An image object is a rectangular array of sample values, each representing a
    color at a particular position within the rectangle. Such objects are typically
    used to represent photographs.
    The painting operators require various parameters, some explicit and others implicit.
    Implicit parameters include the current color, current line width, current
    font (typeface and size), and many others. Together, these implicit parameters
    make up the graphics state; there are operators for setting the value of each implicit
    parameter in the graphics state. Painting operators use the values currently
    in effect at the time they are invoked.
    One additional implicit parameter in the graphics state modifies the results of
    painting graphics objects. The current clipping path outlines the area of the current
    page within which paint can be placed. Although painting operators may
    attempt to place marks anywhere on the current page, only those marks falling
    within the current clipping path affect the page; those falling outside it do not affect
    the page. Initially, the current clipping path encompasses the entire imageable
    area of the page. It can temporarily be reduced to the shape defined by a path
    or text object, or to the intersection of multiple such shapes. Marks placed by subsequent
    painting operators are confined within that boundary.
