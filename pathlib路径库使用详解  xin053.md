# [pathlib路径库使用详解 | xin053](https://xin053.github.io/2016/07/03/pathlib%E8%B7%AF%E5%BE%84%E5%BA%93%E4%BD%BF%E7%94%A8%E8%AF%A6%E8%A7%A3/)

\>>> PureWindowsPath('c:/Program Files/').drive

'c:'

\>>> PurePosixPath('/etc').root

'/'

\>>> p = PureWindowsPath('c:/foo/bar/setup.py')

\>>> p.parents\[0\]

PureWindowsPath('c:/foo/bar')

\>>> p.parents\[1\]

PureWindowsPath('c:/foo')

\>>> p.parents\[2\]

PureWindowsPath('c:/')

\>>> PureWindowsPath('//some/share/setup.py').name

'setup.py'

\>>> PureWindowsPath('//some/share').name

''

\>>> PurePosixPath('my/library/setup.py').suffix

'.py'

\>>> PurePosixPath('my/library.tar.gz').suffix

'.gz'

\>>> PurePosixPath('my/library').suffix

''

\>>> PurePosixPath('my/library.tar.gar').suffixes

\['.tar', '.gar'\]

\>>> PurePosixPath('my/library.tar.gz').suffixes

\['.tar', '.gz'\]

\>>> PurePosixPath('my/library').suffixes

\[\]

\>>> PurePosixPath('my/library.tar.gz').stem

'library.tar'

\>>> PurePosixPath('my/library.tar').stem

'library'

\>>> PurePosixPath('my/library').stem

'library'

\>>> p = PureWindowsPath('c:\\\\windows')

\>>> str(p)

'c:\\\\windows'

\>>> p.as\_posix()

'c:/windows'

\>>> p = PurePosixPath('/etc/passwd')

\>>> p.as\_uri()

'file:///etc/passwd'

\>>> p = PureWindowsPath('c:/Windows')

\>>> p.as\_uri()

'file:///c:/Windows'

\>>> PurePath('a/b.py').match('\*.py')

True

\>>> PurePath('/a/b/c.py').match('b/\*.py')

True

\>>> PurePath('/a/b/c.py').match('a/\*.py')

False

\>>> p = PurePosixPath('/etc/passwd')

\>>> p.relative\_to('/')

PurePosixPath('etc/passwd')

\>>> p.relative\_to('/etc')

PurePosixPath('passwd')

\>>> p.relative\_to('/usr')

Traceback (most recent call last):

  File "<stdin>", line 1, in <module>

  File "pathlib.py", line 694, in relative\_to

    .format(str(self), str(formatted)))

ValueError: '/etc/passwd' does not start with '/usr'

\>>> p = PureWindowsPath('c:/Downloads/pathlib.tar.gz')

\>>> p.with\_name('setup.py')

PureWindowsPath('c:/Downloads/setup.py')

\>>> p = PureWindowsPath('c:/')

\>>> p.with\_name('setup.py')

Traceback (most recent call last):

  File "<stdin>", line 1, in <module>

  File "/home/antoine/cpython/default/Lib/pathlib.py", line 751, in with\_name

raise ValueError("%r has an empty name" % (self,))

ValueError: PureWindowsPath('c:/') has an empty name

\>>> p = PureWindowsPath('c:/Downloads/pathlib.tar.gz')

\>>> p.with\_suffix('.bz2')

PureWindowsPath('c:/Downloads/pathlib.tar.bz2')

\>>> p = PureWindowsPath('README')

\>>> p.with\_suffix('.txt')

PureWindowsPath('README.txt')
