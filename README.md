# volgactf.final
[![PyPI](https://img.shields.io/pypi/v/volgactf.final.svg?style=flat-square)](volgactf.final)
[![PyPI - License](https://img.shields.io/pypi/l/volgactf.final.svg?style=flat-square)](volgactf.final)

[VolgaCTF Final](https://github.com/VolgaCTF/volgactf-final) is an automatic checking system (ACS) for A/D CTF contests.

This repository contains a CLI & public API library for Python 2/3.

## Installation
```
$ pip install volgactf.final
```

## Flag API
### CLI mode
```
$ VOLGACTF_FINAL_API_ENDPOINT=10.0.0.2 volgactf-final flag getinfo 18adda0e7637fe8a3270808222b3a514= 023897b20007996a0563ab92381f38cc=

18adda0e7637fe8a3270808222b3a514=  SUCCESS
  Team: Lorem
  Service: Ipsum
  Round: 1
  Not before: 5/30 16:19:37
  Expires: 5/30 16:24:37
023897b20007996a0563ab92381f38cc=  SUCCESS
  Team: Dolor
  Service: Sit
  Round: 1
  Not before: 5/30 16:19:37
  Expires: 5/30 16:24:37

$ VOLGACTF_FINAL_API_ENDPOINT=10.0.0.2 volgactf-final flag submit 18adda0e7637fe8a3270808222b3a514= 023897b20007996a0563ab92381f38cc=

18adda0e7637fe8a3270808222b3a514=  SUCCESS
023897b20007996a0563ab92381f38cc=  SUCCESS
```

**Note:** 10.0.0.2 stands for an IP address of ACS. You may specify FQDN as well.

You can submit several flags at once. Please take flag API rate limits into consideration.

### Library mode
```python
from volgactf.final.flag_api import FlagAPIHelper

h = FlagAPIHelper('10.0.0.2')
flags = [
    '18adda0e7637fe8a3270808222b3a514=',
    '023897b20007996a0563ab92381f38cc='
]

r1 = h.getinfo(*flags)
# [{'flag': '18adda0e7637fe8a3270808222b3a514=', 'code': <GetinfoResult.SUCCESS: 0>, 'exp': datetime.datetime(2018, 5, 30, 16, 24, 37, tzinfo=tzlocal()), 'service': u'Ipsum', 'team': u'Lorem', 'round': 1, 'nbf': datetime.datetime(2018, 5, 30, 16, 19, 37, tzinfo=tzlocal())}, {'flag': '023897b20007996a0563ab92381f38cc=', 'code': <GetinfoResult.SUCCESS: 0>, 'exp': datetime.datetime(2018, 5, 30, 16, 24, 37, tzinfo=tzlocal()), 'service': u'Sit', 'team': u'Dolor', 'round': 1, 'nbf': datetime.datetime(2018, 5, 30, 16, 19, 37, tzinfo=tzlocal())}]

r2 = h.submit(*flags)
# [{'flag': u'18adda0e7637fe8a3270808222b3a514=', 'code': <SubmitResult.SUCCESS: 0>}, {'flag': u'023897b20007996a0563ab92381f38cc=', 'code': <SubmitResult.SUCCESS: 0>}]
```

Result codes are specified in `volgactf.final.flag_api.GetinfoResult` and `volgactf.final.flag_api.SubmitResult` enums.

## Capsule API
### CLI mode
```
$ VOLGACTF_FINAL_API_ENDPOINT=10.0.0.2 volgactf-final capsule public_key

SUCCESS
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE6O4HeeDG/p7CYoHrDh54SBV2RoYW
oOvajNCsb0tBWPC6VZK2jTFhwzShgAnkwkUvzZMMdDiSmHCZOm5x6KZ25Q==
-----END PUBLIC KEY-----

$ VOLGACTF_FINAL_API_ENDPOINT=10.0.0.2 volgactf-final capsule decode eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NiJ9.eyJmbGFnIjoiZTI0MWNhZDgwZmE1YzFlZGVlYTE1ZjllNjc4YWU4OTA9In0.5lRNzKi_EPcT_wm6i8X0uhwSrV8y8JW0HAATC0dURV8WIEkHsYWoDACd4laaqWdzkS8No-2QREvEF4f5eg4HFw

SUCCESS
  Flag: e241cad80fa5c1edeea15f9e678ae890=
```

### Library mode
```python
from volgactf.final.capsule_api import CapsuleAPIHelper

h = CapsuleAPIHelper('10.0.0.2')

r1 = h.get_public_key()
# {'public_key': u'-----BEGIN PUBLIC KEY-----\nMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE6O4HeeDG/p7CYoHrDh54SBV2RoYW\noOvajNCsb0tBWPC6VZK2jTFhwzShgAnkwkUvzZMMdDiSmHCZOm5x6KZ25Q==\n-----END PUBLIC KEY-----\n', 'code': <GetPublicKeyResult.SUCCESS: 0>}

r2 = h.decode('eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NiJ9.eyJmbGFnIjoiZTI0MWNhZDgwZmE1YzFlZGVlYTE1ZjllNjc4YWU4OTA9In0.5lRNzKi_EPcT_wm6i8X0uhwSrV8y8JW0HAATC0dURV8WIEkHsYWoDACd4laaqWdzkS8No-2QREvEF4f5eg4HFw')
# {'decoded': {u'flag': u'e241cad80fa5c1edeea15f9e678ae890='}, 'code': <DecodeResult.SUCCESS: 0>}
```

Result codes are specified in `volgactf.final.capsule_api.GetPublicKeyResult` and `volgactf.final.capsule_api.DecodeResult` enums.

## License
MIT @ [VolgaCTF](https://github.com/VolgaCTF)
