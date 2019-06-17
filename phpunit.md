# PHPUnit

# 事前準備など
```aaa.php
    public function setUp() {
        // 各テスト前に実行   
    }

    public function tearDown() {
        // 各テスト後に実行
    }
```

# fuel

```aaa.php
use PHPUnit\Framework\TestCase;
```

# モック、スタブ

## 1

プリシス環境では使えない

```
class StubTest extends TestCase
{
    public function testStub()
    {
        // SomeClass クラスのスタブを作成します
        $stub = $this->createMock(SomeClass::class);

        // スタブの設定を行います
        $stub->method('doSomething')
             ->willReturn('foo');

        // $stub->doSomething() をコールすると
        // 'foo' を返すようになります
        $this->assertSame('foo', $stub->doSomething());
    }
}
```

## 2

プリシス環境では使えない


```
class StubTest extends TestCase
{
    public function testStub()
    {
        // SomeClass クラスのスタブを作成します
        $stub = $this->getMockBuilder(SomeClass::class)
                     ->disableOriginalConstructor()
                     ->disableOriginalClone()
                     ->disableArgumentCloning()
                     ->disallowMockingUnknownTypes()
                     ->getMock();

        // スタブの設定を行います
        $stub->method('doSomething')
             ->willReturn('foo');

        // $stub->doSomething() をコールすると
        // 'foo' を返すようになります
        $this->assertSame('foo', $stub->doSomething());
    }
}
```


# private static メソッドのテスト

```
class Test_XXXX extends TestCase {
    public function setUp() {
        $this->Class_XXXX = new class {
            public static function __callStatic(string $name, array $args) {
                $rm = new \ReflectionMethod(
                    Class_Name::class, method_name);

                $rm->setAccessible(true);
                return $rm->invokeArgs(null, $args);
            }
        };
    }

    public function test_xxxx() {
        $this->Class_XXXX::add_supplier_order_id();
    }
}
```