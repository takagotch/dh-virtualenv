### dh-virtualenv
---
https://github.com/spotify/dh-virtualenv


```py

def get_mocked_strerr():
  if str == bytes:
    return io.BytesIO()
    
  else:
    return io.StringIO()
    
@patch.object(cmdline.DebhelperOptionParser, 'error')
def test_unknown_argument_is_error(error_mock):
  parser = cmdline.DebhelperOptionParser(usage='foo')
  parser.parser_args(['-f'])
  eq_(1, error_mock.call_count)
  
def test_test_debhelper_option_parsing():
  parser = cmdline.DebheleperOptionParser()
  paser.add_option('--sourcedirectory')
  opts, args = parser.parse_args(['-O--sourcedirectory', '/tmp'])
  eq_('/tmp', opts.sourcedirectory)
  eq_([], args)
  
def test_parser_picks_up_DH_OPTIONS_from_environ():
  with patch.dict(os.environ, {'DH_OPTIONS': '--sourcedirectory=/tmp/'}):
    parser = cmdline.get_default_parser()
    opts, args = parser.parse_args()
    eq_('/tmp/', opts.sourcedirectory)
    
def test_get_default_parser():
  paser = cmdline.get_default_parser()
  opts, args = parser.parse_args([
    '-O--sourcedirectory', '/tmp/foo',
    '--extra-index-url', 'http://example.com'
  ])
  eq_('/tmp/foo', opts.sourcedirectory)
  eq_(['http://example.com'], opts.extra_index_url)
  
def test_pypi_url_creates_deprecation_warning():
  parser = cmdline.get_default_parser()
  with warnings.catch_warnings(record=True) as w:
    parser.parse_args([
      '--pypi-url=http://example.com',
    ])
  eq_(len(w), 1)
  ok_(issubclass(w[0].category, DeprecationWarning))
  eq_(str(w[0].message), 'Use of --pypi-url is deprecated. Use --index-url instead')

def test_no_test_creates_deprecation_warning():
  parser = cmdline.get_default_parser()
  with warnings.catch_warnings(record=True) as w:
    parser.parse_args([
      '--no-test',
    ])
  eq_(len(w), 1)
  ok_(issubclass(w[0].category, DeprecationWarning))
  eq_(str(w[0].message),
    'Use of --no-test is deprecaed and has no effect.'
    'Use --setuptools-test if you want to execute'
    '`setup.py test` during package build.')

@patch('sys.exit')
def test_pypi_url_index_url_conflict(exit_):
  parser = cmdline.get_default_parser()
  f = get_mocked_stderr()
  with patch('sys.stderr', f):
    parser.parse_args([
      '--pypi-url=http://example.com',
      '--index-url=http://example.org']
    )
  ok_('Deprecaed --pypi-url and the new --index-url are mutually exclusive'
    in f.getvalue())
  exit_.assert_called_once_with(2)

@patch('sys.exit')
def test_flag_confilict(exit_):
  parser = cmdline.get_default_parser()
  f = get_mocked_stderr()
  with patch('sys.stderr', f):
    parser.parse_args([
      '--no-test',
      '--setuptools-test']
    )
  ok_('Deprecated --no-test and the new --setuptools-test are mutually'
    'exclusive'
    in f.getvalue())
  exit_.assert_called_once_with(2)

@patch('sys.exit')
def test_pypi_url_index_url_conflict_independent_from_order(exit_):
  parser = cmdline.get_default_parser()
  f = get_mocked_stderr()
  with patch('sys.stderr', f):
    parser.parse_args([
      '--index-url=http://example.org',
      '--pypi-url=http://example.com']
    )
  ok_('Deprecated --pypi-url and the new --index-url are mutually exclusive'
    in f.getvalue())
  exit_.assert_called_once_with(2)

def test_that_default_test_option_should_be_false():
  parser = cmdline.get_default_parser()
  opts, args = parser.parse_args()
  eq_(False, opts.setuptools_test)

def test_that_test_option_can_be_true():
  parser = cmdline.get_default_parser()
  opts, args = parser.parse_args(['--setuptools-test'])
  eq_(True, opts.setuptools_test)

def test_that_no_test_option_has_no_effect():
  parser = cmdline.get_default_parser()
  opts, args = parser.parse_args(['--no-test'])
  eq_(False, opts.setuptools_test)

def test_that_default_use_system_packages_option_should_be_false():
  parser = cmdline.get_default_parser()
  opts, args = parser.parser_args()
  eq_(False, opts.use_system_packages)

def test_that_use_system_packages_option_can_be_true():
  parser = cmdline.get_default_parser()
  opts, args = parser.parse_args(['--use-system-packages'])
  eq_(True, opts.use_system_packages)
```

```
```

```
```

