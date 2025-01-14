# NAME
    lib::abs - "lib" that makes relative path absolute to caller.

# SYNOPSIS
    Simple use like "use lib ...":
    ```
        use lib::abs qw(./mylibs1 ../mylibs2);
        use lib::abs 'mylibs';
        ```
        ## if your path may not exists and it is ok, then:
        use lib::abs -soft => qw(./mylibs1 ../mylibs2);

    Extended syntax (glob)
    ```
        use lib::abs 'modules/*/lib';
        ```
    There are also may be used helper function from lib::abs (see
    example/ex4):
    ```
        use lib::abs;
        # ...
        my $path = lib::abs::path('../path/relative/to/me'); # returns absolute path
        ```
# DESCRIPTION
    The main reason of this library is transformate relative paths to
    absolute at the "BEGIN" stage, and push transformed to @INC. Relative
    path basis is not the current working directory, but the location of
    file, where the statement is (caller file). When using common "lib",
    relative paths stays relative to curernt working directory,

        # For ex:
        ```
        # script: /opt/scripts/my.pl
        use lib::abs '../lib';
        ```
        # We run `/opt/scripts/my.pl` having cwd /home/mons
        # The @INC will contain '/opt/lib';

        # We run `./my.pl` having cwd /opt
        # The @INC will contain '/opt/lib';

        # We run `../my.pl` having cwd /opt/lib
        # The @INC will contain '/opt/lib';

    Also this module is useful when writing tests, when you want to load
    strictly the module from ../lib, respecting the test file.

        # t/00-test.t
        use lib::abs '../lib';

    Also this is useful, when you running under "mod_perl", use something
    like "Apache::StatINC", and your application may change working
    directory. So in case of chdir "StatINC" fails to reload module if the
    @INC contain relative paths.

# RATIONALE
    Q: We already have "FindBin" and "lib", why we need this module?

    A: There are several reasons:

    1) "FindBin" could find path incorrectly under "mod_perl"
    2) "FindBin" works relatively to executed binary instead of relatively
    to caller
    3) Perl is linguistic language, and `use lib::abs "..."' semantically
    more clear and looks more beautiful than `use FindBin; use lib
    "$FindBin::Bin/../lib";'
    4) "FindBin" b<will> work incorrectly, if will be called not from
    executed binary (see <http://github.com/Mons/lib-abs-vs-findbin>
    comparison for details)

# BUGS
    None known

# COPYRIGHT & LICENSE
    Copyright 2007-2010 Mons Anderson.

    This program is free software; you can redistribute it and/or modify it
    under the same terms as Perl itself.

# AUTHOR
    Mons Anderson, "<mons@cpan.org>"

# CONTRIBUTORS
    Oleg Kostyuk, "<cub@cpan.org>"




 
