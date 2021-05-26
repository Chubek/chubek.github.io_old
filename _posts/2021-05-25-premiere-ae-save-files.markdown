---
layout: post
title:  "Text Inside Adobe Premiere Save Files: What Are They?"
date:   2021-05-25 09:12:17 +0330
categories: interesting
---

This blog has been inactive for months, mostly because I've been working projects that I couldn't talk about (I'm hired by ROSA to make a translator that works with all the Ampersands) but lately a project has befallen me that, not only interesting, is something that trascends my usual "Google and Find Out" fair. 

What is this project? Extracting all the texts inside an Adobe Premiere Project file, listing them for the user, providing automatic translation for them, replacing the text, recompiling the save file, and rendering it. Pretty interesting, huh?

Well, there are some huge problems on the way. First, Premiere Pro files are not text data, they're binary. They are gzipped XML files. I've realized that you can simply change the extension to zip and extract it in using Python's zipfile (although most times I just use Windows as right now we're in the research phase) and the Premiere can open the textual XML file with no issue --- no need to gzip it again. Had I not learned this, I could have killed myself right in front of the monitor. But thankfully I found this out.

Premiere's text data are encoded with base64. They are stored like this:

```
<FormattedTextData Encoding="base64" BinaryHash="645f73fa-27da-41f5-3713-4fd2000001a8">kAEAAAAAAABEMyIREAAAAAwAEAAEAAAACAAMAAwAAABYAAAACAAAABAAAAAAAAAACAAaAAQADAAIAAAASQxpb5LTH1R/wxyBAAAE0wAAAAAAACYALAAIAAQAAAAAACgAJAAAAAAAAAAAACAAHwAYAAAAFAAQAAwAJgAAAEgAAABYAAAAAABAQQAAwEAAAEBAAADIQgAAAAEkAAAAAgAAAAIAAABs////BAAGAAQAAAAAAAoACAAFAAYABwAKAAAAAAAAAAEAAAAEAAAABgAAAFRhaG9tYQAAAQAAAAwAAAAIAAwABAAIAAgAAABUAAAANAAAADAADAAAAAgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEADAAAAAIAAAAAABAQvj////8////BAAEAAQAAABXAAAAQSBuZXcgd29ybGQgZGF3bmVkIG9uIHRoZSBhd2Vzb21lIGFuZCBmcmVlIGNvdW50cnkgb2YgVGVyYmV0aXN0YW4uDUFuZCBoZXJlIGNvbWVzIGxpbmUgMi4A
		</FormattedTextData>
```

The tag differs based on the type of the text. There are three types of text in Premiere:

1. Text layers based on MOGRTs
2. Text layers using text tool
3. Captions

With MOGRT files, the bytes that are generated after decoding the base64 in turn decode into a UTF-16 Little Endian JSON, like this:

```
{'capPropFontEdit': False,
 'capPropFontFauxStyleEdit': False,
 'capPropFontSizeEdit': False,
 'capPropTextRunCount': 1,
 'fontEditValue': ['Helvetica'],
 'fontFSAllCapsValue': [False],
 'fontFSBoldValue': [False],
 'fontFSItalicValue': [False],
 'fontFSSmallCapsValue': [False],
 'fontSizeEditValue': [122],
 'fontTextRunLength': [10],
 'textEditValue': 'éèüčāßäöüç'}
 ```

 If we wish to change the text, we must also change the length. After that we encode it back into base64 and replace it inside the XML file. Parse the XML, but don't unparse it. Just replace the text. If you unparse the XML, the order of the tags will be destroyed, and Premiere can't open it.

 Now, the text of the text layer tool has little rhyme of reason, and we can't decode it with UTF-16LE, we have to decode it with UTF-8. There's a bunch of unwanted characters inside the bytecode that can't be encoded with UTF-8 so we have to ignore the errors. That's just for viewing the string though.

 ```
 �������D3"������������<�����������������w:a�����������������4����TD�D���������Helvetica-Bold������������$�������������VC��������HELLO WORLD!����
```

If we wish to extract the text we need to convert the bytes, as they are, to string and use RegEx to find Latin Text inside it:

```
b'\xd8\x00\x00\x00\x00\x00\x00\x00D3"\x11\x10\x00\x00\x00\x0c\x00\x10\x00\x04\x00\x00\x00\x08\x00\x0c\x00\x0c\x00\x00\x00<\x00\x00\x00\x08\x00\x00\x00\x10\x00\x00\x00\x00\x00\x00\x00\x08\x00\x18\x00\x04\x00\x0c\x00\x08\x00\x00\x00\x0e\x04w\xa7:\xdc\xea\xa4a\xaa\x9f\xf3\x00\x00\x01\xb1\x00\x00\x00\x00\x0c\x00\x14\x00\x08\x00\x04\x00\x10\x00\x0c\x00\x0c\x00\x00\x00\x1c\x00\x00\x004\x00\x00\x00\x00\x80TD\x00\xa0\xd6D\xa8\xff\xff\xff\xac\xff\xff\xff\xb0\xff\xff\xff\x01\x00\x00\x00\x04\x00\x00\x00\x0e\x00\x00\x00Helvetica-Bold\x00\x00\x01\x00\x00\x00\x0c\x00\x00\x00\x08\x00\x0c\x00\x04\x00\x08\x00\x08\x00\x00\x00$\x00\x00\x00\x0c\x00\x00\x00\x08\x00\x08\x00\x00\x00\x04\x00\x08\x00\x00\x00\x00\x00VC\xfc\xff\xff\xff\x04\x00\x04\x00\x04\x00\x00\x00\x0c\x00\x00\x00HELLO WORLD!\x00\x00\x00\x00'
```

Captions are pretty similar, except they look like this decoded to UTF-8:

```
������D3"������������X�����������������IioT��������&�,������(�$��������� ��������&���H���X�����@A��@��@@��B���$���������l�������
����
And here comes line 2.�
```
I've noticed that the first line of the caption can only be extracted using either Unicodedata module or RegEx, but thee second like is pretty visible. A mixtue of the two is necessary.

I'm about to start writing the backend for this service and I've been already paid 250 USD for the first month. It will take less than two months though. What's sweet about this deal is that my client has promised me 20% of the profits of the service, so I can be assured that I'll see returns for the forseeable future.

I have a question from my readers. Due to high amount of Bitcoin miners in my city and the fact that the country's economy is reliant on Bitcoin, they have started rationing the electricity. Three times per week, for total of six hours, the electricity is gone. I need a UPS. Any suggestions? Tell me on Discord Chubak#7400.

Thanks for reading.