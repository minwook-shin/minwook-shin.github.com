---
layout: post
title: Python 기본 테스트 프레임워크 unittest 라이브러리 알아보기
---

오늘은 Python에서 기본적으로 존재하는 테스트 프레임워크인 unittest 라이브러리를 알아보려 합니다.

## unittest 설치

파이썬에 기본적으로 설치되어 있습니다.

## 예제

```python
import unittest
```

unittest를 가져옵니다.

```python
class TestMethods(unittest.TestCase):

    def setUp(self):
        print("set up!")

    def tearDown(self):
        print("Cleaning job")
```

테스트 케이스는 unittest.TestCase를 상속받아서 생성할 수 있습니다.

setUp은 테스트 메소드가 수행되기 전에 동작하며, tearDown은 테스트 메소드가 수행되고 난 뒤에 동작합니다.

```python
    def test_upper(self):
        self.assertEqual('hello'.upper(), 'HELLO')
```

assertEqual로 두 인자가 서로 같은지 확인 할 수 있습니다.

```python
    def test_isupper(self):
        self.assertTrue('HELLO'.isupper())
        self.assertFalse('hello'.isupper())
```

assertTrue로 참인지, assertFalse로 거짓인지 확인할 수 있습니다.

```python
    def test_even(self):
        for i in range(0, 6):
            with self.subTest(i=i):
                self.assertEqual(i % 2, 0)
```

만약 반복문이 테스트 메소드에 포함되어 있다면, subTest로 반복되는 테스트를 구별할 수 있습니다.

subTest로 인하여 오류가 생겼을 때에 분리되어 출력됩니다.

```python
    @unittest.skip("skipping")
    def test_nothing(self):
        self.assertTrue(True)
```

@unittest.skip으로 테스트를 넘길 수 있습니다.

```python
if __name__ == '__main__':
    unittest.main()
```

만약 테스트를 다 작성했다면 메인 함수에서 모든 테스트를 수행할 수 있습니다.

```python
def suite():
    suite = unittest.TestSuite()
    suite.addTest(TestMethods('test_upper'))
    suite.addTest(TestMethods('test_isupper'))
    return suite
```

특정 테스트 메소드들을 묶어서 수행할 수 있습니다.

```python
if __name__ == '__main__':
    runner = unittest.TextTestRunner()
    runner.run(suite())
```

테스트 메소드를 묶어놓은 TestSuite를 수행할 수 있습니다.

```python
def testSomething():
    assert 0 is not None

if __name__ == '__main__':
    testcase = unittest.FunctionTestCase(testSomething)
    testcase.run()
```

만약 기존의 테스트 함수가 존재한다면 FunctionTestCase로 감싸서 테스트 케이스 객체를 사용할 수 있습니다.

