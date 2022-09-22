2021-12-31  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* doc/{gmdoc2html, gmdocselect, imdoc2man, imdocselect}: Fixes to
        work better with both GNU sed and Solaris/Illumos sed.

        \* doc/GNUmakefile: Use GNU make rules to produce the full imdoc
        list for man, html, and tex, rather than relying on a shell
        wildcard expression, since the order produced by the shell
        wildcard expression is indeterminate.


2021-12-30  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* www/index.rst: Document that
        https://graphicsmagick.sourceforge.io/index.html is also
        available.

        \* www/programming.rst: Update URLs.  Add a link to a 'Go' binding.

        \* Copyright.txt: Update Copyright text for 2022 and prepare for
        new year.

2021-12-29  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/{gif.c, meta.c, miff.c, mpc.c, pdb.c, pnm.c xpm.c}: Rely
        on \_MagickReallocateResourceLimitedMemory() to extend memory
        allocation.

        \* magick/memory.c (\_MagickReallocateResourceLimitedMemory): Remove
        size limit from realloc binary expansion.  Fix reallocs tally counts.

        \* magick/memory-private.h(\_MagickReallocateResourceLimitedMemory)
        : Remove use of GCC/Clang '\_\_attribute\_malloc\_\_' since it is not
        appropriate for this function.

        \* magick/memory.c (\_MagickReallocateResourceLimitedMemory):
        Maintain a tally of the total number of octets moved by realloc.

2021-12-27  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/mat.c (ReadMATImage): Change 'ldblk' to size\_t and
        check related calculations for overflow and to avoid possible
        negative seek offsets.
        (FixLogical): Pass 'ldblk' as size\_t.
        (ReadMATImage): Assure that corrupt/incomplete image is not
        returned.

2021-12-25  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/profile.c (AppendImageProfile): Deprecate this function
        since JPEG was the last user.

        \* coders/jpeg.c (AppendProfile): Implement more efficient way to
        append JPEG profile chunks.  Addresses SourceForge issue #634
        "Slow to read JPEG with big APP1 profile".

        \* coders/jp2.c (ReadJP2Image): Free 'options' which is now
        allocated memory.

        \* coders/png.c (ReadOnePNGImage): Support the define
        png:chunk-malloc-max=limit in order to allow reading PNG files
        which report "chunk data is too large" or to reduce the default
        limit.  The default libpng limit is 8,000,000 bytes, which is more
        than sufficient for normal files.  Note that since PNG uses
        compression, the limit is on the uncompressed data and a
        relatively small file may be able to provide a chunk which
        decompresses to a large size.  Addresses SourceForge #594 "Be able
        to affect libpng user\_chunk\_malloc\_max".

2021-12-24  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* PerlMagick/Magick.xs: Fix issue that image fill attribute had
        its opacity reset to transparent so it could not be usefully set
        at image scope.  Note that this fix may change results if the fill
        attribute is not set since then the default will actually be used.
        Support Get of "fill" and "stroke".  Addresses SourceForge issue
        #650 "Image attributes not working and not as documented for
        PerlMagick".  Eliminate use of dangerous strncpy().

        \* magick/transform.c (TransformImage): Trace crop geometry. Trace
        transform geometry.
        (CropImage): Trace crop geometry. Trace bounding page.

        \* Magick++/lib/Image.cpp (Magick::Image::trim): Trim requires
        NorthWestGravity.

        \* magick/command.c (MogrifyImage): Trim requires
        NorthWestGravity. Fixes SourceForge issue #653 "convert: warning
        when using -gravity with -trim".  This was more than a warning.

2021-12-23  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jp2.c: Properly support passing JasPer options for
        decoder and encoder, including the 'debug' option.
        (initialize\_jasper): Call jas\_conf\_set\_allocator() and pass the
        maximum amount of memory it should be allocated to use.  This
        seems to avoid a malfunction.  Re-enable use of jas\_initialize().

2021-12-22  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jp2.c (realloc\_rlm): JasPer wants its custom memory
        allocators to return non-null for a zero-sized request.  Make it
        so for the 'realloc' case as well.  Development JasPer 3.0.0
        jas\_initialize() is not yet ready for our purposes.

        \* coders/mat.c (ReadMATImageV4): Change 'ldblk' to size\_t and
        check related calculations for overflow and to avoid possible
        negative seek offsets.

2021-12-21  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/signature.c: Remove functions FinalizeSignature,
        GetSignatureInfo, TransformSignature, UpdateSignature and other
        private implementation details from signature.h.  Use managed
        memory allocator.

        \* coders/png.c (DestroyJNG): Deallocate alpha\_image temporary file in DestroyJNG().
        (ReadOnePNGImage): If setjmp fires, then don't return a broken
        image.

2021-12-18  Fojtik Jaroslav  <JaFojtik@yandex.com>

        \* magick/wpg.c: Fix incorrect TrX and TrY elements in CTM.
        Backported from WP2LaTeX: https://hg.osdn.net/view/wp2latex/wp2latex/rev/ce47149e7e67

2021-12-14  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* utilities/tests/convert.tap: Use sed to count file lines rather
        than wc since wc output is inconsistent.  Fixes SourceForge issue
        #657 "Convert test fails in 1.3.37".

        \* configure.ac: Search for a GnuPG 'gpg' program.

        \* Makefile.am (snapshot): Add support for PGP-signed snapshots.

        \* www/download.rst: Add documentation for how to validate
        downloaded files.

2021-12-12  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* version.sh: Update shared library versioning information for
        forthcoming 1.3.37 release.

        \* scripts/graphicsmagick\_snapshot\_copy-ssh: Sample
        graphicsmagick\_snapshot\_copy script which uses scp to copy
        snapshot files to a directory at a remote host, and then rsync
        over ssh from there to SourceForge.  Copy to a local script
        directory and edit to suit local requirements.

        \* scripts/graphicsmagick\_snapshot\_copy-local: Sample
        graphicsmagick\_snapshot\_copy script to copy snapshot files to a
        local directory and then rsync over ssh to SourceForge.  Copy to a
        local script directory and edit to suit local requirements.

        \* Makefile.am (snapshot): Rely on the graphicsmagick\_snapshot\_copy
        script to reduce files we don't care to distribute so that all
        targets are still produced.

        \* coders/jp2.c (alloc\_rlm): JasPer expects its allocator to return
        non-null for zero size

2021-12-05  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* Makefile.am (snapshot): Update the snapshot target to distribute
        a minimal set of files.

        \* magick/symbols.h: Update the list of Gm prefixed symbols.

        \* NEWS.txt: Updated with changes until today.

2021-12-04  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* www/index.rst: Development snapshots are now at SourceForge.

        \* www/download.rst, www/index.rst, README.txt: Remove references
        to ftp.graphicsmagick.org, which was shut down due to continuing
        abusive practices and lack of support from the user community.

2021-12-03  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/memory-private.h
        (MagickAllocateResourceLimitedMemoryAttribute): Remove extraneous
        comma.

        \* coders/wpg.c (LoadWPG2Flags): Fix comment type and whitespace
        issues.

2021-11-15  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/caption.c (ReadCAPTIONImage): Set draw\_info to NULL upon
        deallocation.  Fixes an assertion reported by Michael Melcher via
        the graphicsmagick-bugs list on November 11, 2021.

2021-11-05  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jp2.c (initialize\_jasper): For JasPer 3.0.0 and later,
        use resource-limited memory allocators. JasPer 3.0.0 is not yet
        released at this time.

2021-11-04  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/gif.c (ReadGIFImage): Handle GIF files where the 'opaque'
        index matches the number of colors by producing an extra colormap
        entry of transparent black. Fixes SourceForge issue 649 "Bug with
        gm identify" where the test case produces the error "Invalid
        colormap index (index 128 >= 128 colors, /tmp/broken.gif)".

        \* magick/enum\_strings.c (StringToDisposeType): New utility
        function to convert a string to a DisposeType.
        (DisposeTypeToString) New utility function to convert a
        DisposeType to a string.

        \* coders/msl.c (MSLEndElement): Ignore imbalanced group
        closure. Fixes oss-fuzz 40680 "graphicsmagick:coder\_MSL\_fuzzer:
        Heap-buffer-overflow in MSLEndElement".

2021-11-03  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/tiff.c (ReadTIFFImage): Make sure that loops using
        TIFFReadScanline(), etc, do quit upon first reported error.  Fixes
        oss-fuzz 39167 "graphicsmagick:coder\_BIGTIFF\_fuzzer:
        Use-of-uninitialized-value in DisassociateAlphaRegion", as well as
        other such cases.

        \* coders/png.c (png\_get\_data): On a short read, assure that the
        remainder of the buffer is initialized just in case subsequent
        code accesses it.

        \* coders/msl.c (MSLStartElement): Assure that
        'msl\_info->attributes[n]' is not NULL before attempting to use it.
        This is assumed to eliminate oss-fuzz 40226
        "graphicsmagick:coder\_MSL\_fuzzer: ASSERT: image != (Image \*)
        NULL".

2021-11-02  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (MSLStartElement): Return immediately if there is
        already an error or the image is NULL.  Do not discard exceptions
        when calling functions which return a new image.  Try even harder
        to shut down the libxml2 parser. Fixes SourceForge issue 652 "SEGV
        in gm at coders/msl.c:883".

2021-10-31  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/widget.c (MagickXPreferencesWidget): Eliminate
        compilation warning about 'strlen' argument missing terminating
        nul.

2021-10-24  Fojtik Jaroslav  <JaFojtik@yandex.com>

        \* magick/wpg.c: ObjectID>=0x8000 automatically switches double precision on.

2021-09-18  Fojtik Jaroslav  <JaFojtik@yandex.com>

        \* PerlMagick/t/wmf/JPGinside.emf: Add test file: EMF that embedds
        JPG.

2021-09-17  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/blob.c (WriteBlob): Use appropriate handle for bzip2.
        Patch by Sam James <sam@gentoo.org>.

2021-08-26  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jp2.c (initialize\_jasper): Make minimal use of new JasPer
        function jas\_initialize() in order to avoid severe problems with
        jas\_init().

        \* configure.ac: Detect new JasPer function jas\_initialize().

2021-08-14  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (MSLError): Call xmlStopParser() rather than
        setting parser instate = XML\_PARSER\_EOF.

        \* coders/svg.c (SVGError): Call xmlStopParser() rather than
        setting parser instate = XML\_PARSER\_EOF.

2021-07-21  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/pdf.c (WritePDFImage): Use appropriate memory deallocator
        for memory returned by StringToList().  Fixes SourceForge issue
        646 "Assertion failed using -label with PDF".

        \* coders/webp.c (ReadWEBPImage): Add full error checking when
        retrieving embedded profiles.

        \* magick/profile.c (SetImageProfile): Do not try to store a
        zero-sized profile.

        \* coders/webp.c (ReadWEBPImage): Enforce that embedded profiles
        provided by libWebP are not zero-sized. This problem was brought
        to our attention by Shane Bishop on the graphicsmagick-help
        mailing list.

2021-07-18  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* Makefile.am: Add support for using an external
        'graphicsmagick\_snapshot\_copy' script to copy files for the
        'snapshot' target.  This provides local control over how files are
        copied and where they are copied to.

        \* coders/msl.c (MSLStartElement): Use macros to simplify
        validations and reduce repeated code fragments. Add validations
        for image size and pixels present where applicable.  Fixes
        oss-fuzz 36224 "graphicsmagick:coder\_MSL\_fuzzer: Timeout in
        coder\_MSL\_fuzzer".

        \* magick/transform.c (RollImage): Assert that image rows and
        columns are not zero.

2021-07-16  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jp2.c (initialize\_jasper): Update for the latest version
        of the evolving jas\_init\_custom() interface provided by the
        mdadams-callbacks branch.

2021-07-05  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jp2.c: Assure that the designated decoder is used rather
        than using autodetection and possibly using a different decoder
        than intended. Added experimental support for JasPer
        HAVE\_JAS\_INIT\_CUSTOM feature, but leave disabled by default.  Fix
        a stream manager bug noticed with the madams-callbacks branch of
        JasPer.

2021-06-27  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (ProcessMSLScript): Fix possible use of freed
        memory.  Fixes oss-fuzz 35621 "graphicsmagick:coder\_MSL\_fuzzer:
        ASSERT: image->signature == MagickSignature".

        \* fuzzing/oss-fuzz-build.sh: Disable reading and writing of
        gzip/bzip files since we don't have a viable solution for formats
        which require an uncompressed file as input.

2021-06-26  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/pcx.c (ReadPCXImage): Fix problem that 16-colors are used
        rather than 256-colors given sample file provided.  Resolves
        SourceForge patch #65 "PCX file not read correctly".  Patch is by
        Sam Yang.

        \* coders/jp2.c (ReadJP2Image): Pass "max\_samples" option to Jasper
        to try to limit the amount of memory it may allocate while opening
        a file.  Addresses oss-fuzz 35265
        "graphicsmagick:coder\_PGX\_fuzzer: Out-of-memory in
        coder\_PGX\_fuzzer".

2021-06-22  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/webp.c (ReadWEBPImage): Use SetImagePixelsEx() rather
        than GetImagePixelsEx() in reader.  Patch by Tobias Mark via
        SourceForge patch #66 "Minor improvment webp".

2021-06-12  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/svg.c (GetTransformTokens): Fix massive over-allocation
        of string due to use of AcquireString() on entire remaining text
        buffer. Addresses oss-fuzz 35171 "graphicsmagick:coder\_SVG\_fuzzer:
        Out-of-memory in coder\_SVG\_fuzzer".
        (GetTransformTokens): Apply an arbitrary limit on number of tokens
        to avoid DOS.
        (GetStyleTokens): Fix massive over-allocation of string due to use
        of AcquireString() on entire remaining text buffer.
        (GetStyleTokens): Don't use strlcpy() to copy token because it
        scans full text.
        (GetTransformTokens): Don't use strlcpy() to copy token because it
        scans full text.

2021-06-05  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (MSLStartElement): Use resource-managed memory
        allocator for msl\_info->group\_info and assure that memory is
        cleared so that empty group does not result in use of
        uninitialized data.  Addresses oss-fuzz 34869
        "graphicsmagick:coder\_MSL\_fuzzer: Use-of-uninitialized-value in
        MSLEndElement".

        \* magick/memory.c (\_MagickReallocateResourceLimitedMemory): Round
        up allocation size on small reallocs in order to lessen the number
        of actual reallocs.

2021-05-31  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/svg.c (SVGComment): Re-implement comment callback to be based on
        the managed-memory allocator and avoid excessive use of strlen().
        (SVGCharacters): Re-implement characters callback to to be based
        on the managed-memory allocator and avoid excessive use of
        strlen().  Addresses oss-fuzz 34168
        "graphicsmagick:coder\_SVGZ\_fuzzer: Timeout in coder\_SVGZ\_fuzzer".

        \* magick/memory.c (\_MagickResourceLimitedMemoryGetSizeAttribute):
        New private function to retrieve various integral size values from
        the managed-memory allocator regarding a specified allocation.

        \* magick/utility.c (MagickStripString): New function to replace
        'Strip' which is now deprecated.  This version returns the string
        length.

2021-05-10  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* Magick++/lib/Magick++/{Drawable.h, STL.h}: Use \_MSVC\_LANG in
        addition to \_\_cplusplus when testing for C++'17 since the
        Microsoft C++ compiler only properly defines \_\_cplusplus if the
        /Zc:\_\_cplusplus switch was provided.

2021-05-09  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* Magick++/lib/Magick++/STL.h: Support compiling with C++'98
        through C++'17.

        \* Magick++/lib/Magick++/Drawable.h: Support compiling with C++'98
        through C++'17.

2021-05-02  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/command.c (CompareImageCommand): If user has not
        indicated a 'matte' preference, then include the opacity channel
        in the compare if either image has a matte channel.  Addresses
        SourceForge issue #642 "Result of command "gm compare" depends on
        order of images".

2021-04-25  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/compare.c (IsImagesEqual): Eliminate hard
        "ImageOpacityDiffers" error when matte channel flag differs
        between the images being compared.  Instead of throwing a hard
        error, treat the opacity channel of the image as opaque if the
        matte flag is not set.

2021-04-24  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jpeg.c: It is apparently now undefined behavior to assign
        the return value from setjmp() to a variable.  Remove recently
        added code which is now doing that.  Much thanks to Chris Gravely
        for noticing this.

2021-04-19  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jpeg.c (ReadJPEGImage): Error cases depending on
        ThrowReaderException() were now leaking client data memory.
        Replace such cases with customized ThrowJPEGReaderException()
        which assures that it is freed.
        (WriteJPEGImage): Error cases depending on ThrowWriterException()
        were now client data memory.  Replace such cases with customized
        ThrowJPEGWriterException() which assures that it is freed.

2021-04-18  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jpeg.c: Restructure client data so it is allocated on the
        heap rather than the stack.  Happens to fix SourceForge issue 641
        "SIGSEGV thrown performing longjmp in jpeg.c".

2021-04-10  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/png.c (ReadOnePNGImage): Assure that null
        ping\_trans\_alpha pointer is not dereferenced.  Addresses oss-fuzz
        33119 "graphicsmagick:coder\_MNG\_fuzzer: Null-dereference READ in
        ReadOnePNGImage".

        \* magick/profile.c (SetImageProfile): Use the resource-limited
        memory allocator to allocate embedded profiles.

        \* magick/map.c (MagickMapCopyResourceLimitedString): New private
        function to copy a resource-limited string.
        (MagickMapDeallocateResourceLimitedString): New private function
        to deallocate a resource-limited string.
        (MagickMapCopyResourceLimitedBlob): New private function to copy a
        resource-limited blob.
        (MagickMapDeallocateResourceLimitedBlob): New private function to
        deallocate a resource-limited blob.

2021-04-06  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/svg.c (GetStyleTokens): Limit the number of style tokens.
        Addresses oss-fuzz 32921 "graphicsmagick:coder\_SVG\_fuzzer:
        Out-of-memory in coder\_SVG\_fuzzer".
        (SVGComment): Only capture first comment rather than concatenating
        all comments.  Addresses oss-fuzz 32944
        "graphicsmagick:coder\_SVGZ\_fuzzer: Timeout in coder\_SVGZ\_fuzzer".

2021-04-02  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (MSLReference): Fix memory leak when parser node is
        null. Addresses oss-fuzz 32713 "graphicsmagick:coder\_MSL\_fuzzer:
        Direct-leak in xmlNewReference".

2021-04-01  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/png.c (ReadOnePNGImage): Avoid use of null
        ping\_trans\_color.  Fixes oss-fuzz 32666
        "graphicsmagick:coder\_MNG\_fuzzer: Null-dereference READ in
        ReadOnePNGImage".

2021-03-29  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (WriteMSLImage): Add OpenBlob()/CloseBlob() which
        seems necessary to avoid memory leak in ImageToBlob().  Hopefully
        will fix oss-fuzz 32575 "graphicsmagick:coder\_MSL\_fuzzer:
        Direct-leak in MagickMalloc".

2021-03-26  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/jp2.c (RegisterJP2Image): Report JasPer library version.

        \* coders/msl.c (ProcessMSLScript): Free msl\_image upon reader
        failure.  Should fix oss-fuzz 32479
        "graphicsmagick:coder\_MSL\_fuzzer: Indirect-leak in MagickMalloc".

2021-03-24  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* PerlMagick/Makefile.am (check-perl): Nullify the check-perl
        target when PerlMagick is enabled and shared libraries are used.
        This is because a dynamic GraphicsMagick needs to be formally
        installed before PerlMagick can be tested.

        \* coders/jp2.c (ReadJP2Image): Support both old and new ways to
        determine if JasPer codec support is available.

        \* coders/msl.c (ProcessMSLScript): Another attempt to properly fix
        oss-fuzz 32263 "graphicsmagick:coder\_MSL\_fuzzer:
        Heap-use-after-free in ProcessMSLScript" without causing new
        problems.

2021-03-23  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (ProcessMSLScript): Fix oss-fuzz 32263
        "graphicsmagick:coder\_MSL\_fuzzer: Heap-use-after-free in
        ProcessMSLScript".

2021-03-18  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* Magick++/lib/Image.cpp (Magick::Image::write): Due to the design
        of ImageToBlob(), it is possible for data to be returned although
        an exception was thrown.  Deposit it in the Blob so that it will
        be freed.  May finish fixing oss-fuzz 31965
        "graphicsmagick:coder\_MSL\_fuzzer: Indirect-leak in MagickMalloc".

2021-03-17  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (ProcessMSLScript): Attempt to address leak of
        "msl\_image".  May fix oss-fuzz 31965
        "graphicsmagick:coder\_MSL\_fuzzer: Indirect-leak in MagickMalloc".

2021-03-13  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/dcm.c (DCM\_ReadNonNativeImages): Enforce that image depth
        is in the supported range of 1-16.  Embedded PGX was observed to
        cause JasPer to report a component depth of 20 bits.  Fixes
        oss-fuzz issue 31373 "graphicsmagick:coder\_DCM\_fuzzer:
        Heap-buffer-overflow in DCM\_SetupRescaleMap".

2021-03-10  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/svg.c (SVGError): Force xml parser input state to
        XML\_PARSER\_EOF state upon error to abort parsing.

        \* coders/msl.c (MSLError): Force xml parser input state to
        XML\_PARSER\_EOF state upon error to abort parsing.  Fixes oss-fuzz
        31401 "graphicsmagick:coder\_MSL\_fuzzer: Timeout in
        coder\_MSL\_fuzzer".

2021-03-08  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (ProcessMSLScript): Replicate clean-up actions
        which should already be done by MSLPopImage().  Trying to address
        oss-fuzz 31259 "graphicsmagick:coder\_MSL\_fuzzer: Direct-leak in
        MagickMalloc", which I have not been able to reproduce.

        \* magick/tsd.c (MagickTsdKeyDelete): Fix memory leak of key values
        array at exit when use of pthread or WIN32 TSD APIs is disabled.

2021-03-07  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (MSLStartElement): Consistently verify that
        attributes are non-NULL before calling TranslateText().  Fixes
        oss-fuzz 31779 "graphicsmagick:coder\_MSL\_fuzzer: ASSERT: image !=
        (Image \*) NULL".

        \* README.txt: Add mention of libdeflate library, since it is an
        optional dependency of the next libtiff release, and might be
        required to link if libtiff itself depends on it.

        \* configure.ac (MAGICK\_DEP\_LIBS): Liblzma is a libtiff dependency.
        GraphicsMagick does not directly use liblzma.  Do not include
        liblzma as direct dependency for the modules build.

2021-03-04  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/svg.c (ProcessStyleClassDefs): Fix non-terminal execution
        while traversing "active" list based on a patch by Gregory J
        Wolfe.  Addresses oss-fuzz 31663
        "graphicsmagick:coder\_SVGZ\_fuzzer: Timeout in coder\_SVGZ\_fuzzer".

2021-03-02  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/svg.c (ProcessStyleClassDefs): Corrected fix for oss-fuzz
        31234 "graphicsmagick:coder\_SVG\_fuzzer: Direct-leak in
        MagickMalloc" based on a patch by Gregory J Wolfe.

2021-02-28  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* configure.ac: Add tests for Jasper jp2\_decode(), jpc\_decode(),
        and pgx\_decode().

        \* coders/jp2.c (ReadJP2Image): Call jp2\_decode(), jpc\_decode(), or
        pgx\_decode(), directly. Using jas\_image\_decode() makes us subject
        to Jasper's own format determination, which may include file
        formats we don't want to support via Jasper.

        \* fuzzing/oss-fuzz-build.sh: Disable support for Jasper codecs we
        don't want or need.

2021-02-27  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (MSLStartElement): Fix assertion in TranslateText()
        when there are no attributes available.  Addresses oss-fuzz 31307
        "graphicsmagick:coder\_MSL\_fuzzer: ASSERT: image != (Image \*)
        NULL".

        \* coders/svg.c (ProcessStyleClassDefs): Fix memory leak upon
        malformed class name list.  Addresses oss-fuzz 31234
        "graphicsmagick:coder\_SVG\_fuzzer: Direct-leak in MagickMalloc".
        (ProcessStyleClassDefs): Fix non-terminal loop and huge memory
        allocation caused by self-referential list.  Not sure if
        implementation is as intended, but it does not crash. Addresses
        oss-fuzz 31391 "graphicsmagick:coder\_SVG\_fuzzer: Out-of-memory in
        coder\_SVG\_fuzzer".
        (SVGReference): Fix memory leak when parser node is null.
        Addresses oss-fuzz 31286 "graphicsmagick:coder\_SVGZ\_fuzzer:
        Direct-leak in xmlNewReference".

2021-02-25  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/msl.c (MSLCDataBlock): Fix leak of value from
        xmlNewCDataBlock().  Addresses oss-fuzz 31400
        "graphicsmagick:coder\_MSL\_fuzzer: Direct-leak in
        xmlNewCDataBlock".

2021-02-22  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/svg.c (ProcessStyleClassDefs): Fix non-terminal loop
        caused by a self-referential list which results in huge memory
        usage.  Addresses oss-fuzz 31238 "graphicsmagick:coder\_SVG\_fuzzer:
        Out-of-memory in coder\_SVG\_fuzzer".

2021-02-21  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/svg.c (SVGStartElement): Reject impossibly small bounds
        and view\_box width or height.  Addresses oss-fuzz 31224
        "graphicsmagick:coder\_SVG\_fuzzer: Divide-by-zero in
        SVGStartElement".

        \* coders/msl.c (MSLPushImage): Only clone attributes if not null.
        Should address oss-fuzz 31205 "graphicsmagick:coder\_MSL\_fuzzer:
        ASSERT: image != (Image \*) NULL".

        \* coders/jp2.c (ReadJP2Image): Validate that actual file header
        does appear to be a supported format regardless of 'magick' being
        forced.  Jasper appears to dispatch to other libraries if it
        detects a known format it supports and then the program exits if
        there is a problem.  Fixes oss-fuzz 31200
        "graphicsmagick:coder\_JPC\_fuzzer: Unexpected-exit in error\_exit".

2021-02-20  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/nt\_base.c (NTGhostscriptFind,NTGhostscriptGetString):
        Handle Ghostscript point versions added after 9.52.  Fixes
        SourceForge issue #636 'Failed to find Ghostscript' with
        Ghostscript version 9.53.0+.

        \* fuzzing/oss-fuzz-build.sh: Patch by Paul Kehrer to incorporate
        Jasper and libxml2 into the oss-fuzz build.

2021-02-14  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* VisualMagick/All/All.vcproj.in: Fixes by sourcer42
        <sourcer42@users.sourceforge.net> for the problem that Visual
        Studio is not able to load the All project if the project supports
        the x64 target.

2021-02-12  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* www/Hg.rst: Document new redundant Mercurial server at OSDN,
        "https://hg.osdn.net/view/graphicsmagick/GM".

2021-02-07  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* Add explicit cast to float where implicit casts to float from
        double were occurring.

        \* magick/utility.c (MagickDoubleToLong): Guard against LONG\_MAX
        not directly representable as a double.

2021-02-06  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/utility.c (TranslateTextEx): If image resolution is
        impossibly small, then report the default resolution of 72 DPI, or
        the equivalent in centimeters if units is in
        pixels-per-centimeter.  Addresses SourceForge bug #396 "dpi not
        retrived (no default value)".  I do have some misgivings about
        this solution since it is lying about the actual value.  Not all
        usages of raster images have an associated physical reality and
        thus resolution is not necessarily relevant.

2021-02-04  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/tiff.c, coders/ps2.c, coders/ps3.c: Libtiff versions
        beyond 20201219 want to use types from stdint.h.

2021-01-31  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/monitor.c (MagickMonitorActive): Need to export this
        function for use by modules.

2021-01-30  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* VisualMagick/bin: Remove hp2xx.exe, mpeg2dec.exe, and
        mpeg2enc.exe.  There is no value to distributing these pre-built
        and flimsy executables in the source package.

        \* filters/analyze.c (AnalyzeImage): Add OpenMP speed-ups.

2021-01-29  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* filters/analyze.c (AnalyzeImage): Tidy the structure of the code
        a bit.

        \* magick/module.c (ExecuteModuleProcess): Add error reporting for
        the case that the expected symbol is not resolved.

2021-01-23  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* configure.ac: Remove updates to use recommended forms of AC\_INIT
        and AM\_INIT\_AUTOMAKE.  There were too many annoying side-effects
        to daily development from these changes.  Perhaps they will be
        re-visited if solutions for Autotools regeneration issues are
        found.

2021-01-19  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* magick/render.c (InverseAffineMatrix): Avoid possible division
        by zero or absurdly extreme scaling in InverseAffineMatrix().
        Fixes oss-fuzz 28293 "Divide-by-zero - InverseAffineMatrix".

2021-01-13  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* configure.ac (CONFIG\_STATUS\_DEPENDENCIES): Regenerate
        configure.ac if ChangeLog or version.sh is updated.

2021-01-10  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/pdf.c (WritePDFImage): Converting a TIF to a PDF set the
        page MediaBox to the TIFF dimensions in pixels while the CropBox
        is set in local context dimensions. The latter is correct, the
        former is not.  Set the MediaBox to the proper dimension in local
        context.  Should be the same in this context.  Patch by Hubert
        Figuiere and retrieved from SourceForge patch #64 "Incorrect
        MediaBox in PDF export".

        \* magick/pixel\_cache.c: Memory cache implementation of pixel cache
        now uses resource limited memory allocator.  It was previously
        resource limited, but by using the resource allocation APIs
        directly.

2021-01-09  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/tiff.c: Remove unintended double-charging for memory
        resource.  Remove explicit memset where possible.

2021-01-07  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* coders/gif.c (ReadGIFImage): Fix memory leak of global\_colormap
        if realloc of memory for comment fails.  Fixes oss-fuzz 29316
        "Direct-leak in MagickMalloc".

        \* coders/meta.c (ReadMETAImage): Fix double-free if blob buffer
        was reallocated after being attached to blob.  Fixes oss-fuzz
        29193 "Heap-double-free in MagickFree".

2021-01-06  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* configure.ac: Updates to use recommended forms of AC\_INIT and
        AM\_INIT\_AUTOMAKE.  This was/is painful due to how development
        snapshot versioning is handled.  The version string produced for
        the snapshot version will now contain the snapshot date.  Effort
        has been made to avoid other impacts due to AC\_INIT's enforcements
        for how version information is used.

2021-01-02  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* PerlMagick/Magick.xs: Remove GCC warnings which spewed out at
        increased warning levels.

        \* magick/magick\_types.h.in: Hide definitions not intended for the
        rest of the world under "if defined(MAGICK\_IMPLEMENTATION)".

2021-01-01  Bob Friesenhahn  <bfriesen@simple.dallas.tx.us>

        \* configure.ac: Skip library symbol tests for gdi32 since these
        fail with the MSYS2 w64-i686 compiler and well as i686 Cygwin.
        The failures caused a build regression for i686 MSYS2/Cygwin.

        \* Copyright.txt: Copyright year updates and ChangeLog rotation for
        the new year.
