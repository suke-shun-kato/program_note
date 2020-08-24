# PHPUnit

# 事前準備など
```
    public function setUp() {
        // 各テスト前に実行   
    }

    public function tearDown() {
        // 各テスト後に実行
    }
```

# fuel

```
use PHPUnit\Framework\TestCase;
```

# assert

https://qiita.com/rev84/items/12fbd16d210d6a86eff9

## 配列

### 全要素を検証

```
    private function assertArrayElement(array $expected_ary, array $actual_ary) {
        foreach ($expected_ary as $expected) {
            $this->assertContains($expected, $actual_ary);
        }
        $this->assertCount(count($expected_ary), $actual_ary);
    }
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
                     ->disallowMockingUnknownTypes()    // プリシス環境ではこのメソッドは使えない
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

`ABC` クラスの `def` メソッドの場合

```
class Test_ABC_def extends TestCase {

    private $ABC;
    
    public function setUp() {
        $this->ABC = new class {
            public static function __callStatic(string $name, array $args) {
                $rm = new \ReflectionMethod(
                    ABC::class, 'def');

                $rm->setAccessible(true);
                return $rm->invokeArgs(null, $args);
            }
        };
    }

    public function test_xxxx() {
        $this->ABC::def();
    }
}
```