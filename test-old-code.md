## Testing old, non-class php code

[Origin post](https://stackoverflow.com/questions/899390/how-do-i-write-unit-tests-in-php-with-a-procedural-codebase)

You can unit-test procedural PHP, no problem. And you're definitely not out of luck if your code is mixed in with HTML.

At the application or acceptance test level, your procedural PHP probably depends on the value of the superglobals ($_POST, $_GET, $_COOKIE, etc.) to determine behavior, and ends by including a template file and spitting out the output.

To do application-level testing, you can just set the superglobal values; start an output buffer (to keep a bunch of html from flooding your screen); call the page; assert against stuff inside the buffer; and trash the buffer at the end. So, you could do something like this:

```
public function setUp()
{
    if (isset($_POST['foo'])) {
        unset($_POST['foo']);
    }
}

public function testSomeKindOfAcceptanceTest()
{
    $_POST['foo'] = 'bar';
    ob_start();
    include('fileToTest.php');
    $output = ob_get_flush();
    $this->assertContains($someExpectedString, $output);
}
```

Even for enormous "frameworks" with lots of includes, this kind of testing will tell you if you have application-level features working or not. This is going to be really important as you start improving your code, because even if you're convinced that the database connector still works and looks better than before, you'll want to click a button and see that, yes, you can still login and logout through the database.

At lower levels, there are minor variations depending on variable scope and whether functions work by side-effects (returning true or false), or return the result directly.

Are variables passed around explicitly, as parameters or arrays of parameters between functions? Or are variables set in many different places, and passed implicitly as globals? If it's the (good) explicit case, you can unit test a function by (1) including the file holding the function, then (2) feeding the function test values directly, and (3) capturing the output and asserting against it. If you're using globals, you just have to be extra careful (as above, in the $_POST example) to carefully null out all the globals between tests. It's also especially helpful to keep tests very small (5-10 lines, 1-2 asserts) when dealing with a function that pushes and pulls lots of globals.

Another basic issue is whether the functions work by returning the output, or by altering the params passed in, returning true/false instead. In the first case, testing is easier, but again, it's possible in both cases:

```
// assuming you required the file of interest at the top of the test file
public function testShouldConcatenateTwoStringsAndReturnResult()
{
  $stringOne = 'foo';
  $stringTwo = 'bar';
  $expectedOutput = 'foobar';
  $output = myCustomCatFunction($stringOne, $stringTwo);
  $this->assertEquals($expectedOutput, $output);
}
```
In the bad case, where your code works by side-effects and returns true or false, you still can test pretty easily:

```
/* suppose your cat function stupidly 
 * overwrites the first parameter
 * with the result of concatenation, 
 * as an admittedly contrived example 
 */
public function testShouldConcatenateTwoStringsAndReturnTrue()
    {
      $stringOne = 'foo';
      $stringTwo = 'bar';
      $expectedOutput = 'foobar';
      $output = myCustomCatFunction($stringOne, $stringTwo);
      $this->assertTrue($output);
      $this->Equals($expectedOutput, $stringOne);
    }
```
Hope this helps.
