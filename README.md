# Signal (Alternative Observer pattern)

Signal is one of techniques to observe changes in Scala's Reactive programming. 
When compares to Pub/Sub design pattern. Signal is much cleaner implementation.

###### Signal will maintain 3 properties

- Its current value
- Its expression(Scala's block) to evaluate
- A Set of observers which will be evaluated once the Signal's current value changed

### Usage Example

```Scala
import ionsap.signal._ 
 
object Example {
  def main(args: Array[String]): Unit = {
    /**
     * Declare mutable Signals
     */
    val mutableSig1 = Var(5)
    val mutableSig2 = Var(10)
    
    /**
     * Declare immutable Signals
     */
    val immutableSig3 = Signal{mutableSig1() + mutableSig1()}
    val immutableSig4 = Signal(5.5)

    /**
     * Access value of mutable Signals
     */
    println(mutableSig1())
    println(mutableSig2())
    println(immutableSig3())
    println(immutableSig4())
    
    /**
     * java.lang.AssertionError: cyclic signal definition
     */
    mutableSig1() = mutableSig1() + 1
    
    /**
     * Fixed version for updating Signal values
     */
    val tmpSig1 = mutableSig1()
    mutableSig1() = tmpSig1 + 1
    val tmpSig2 = mutableSig2()
    mutableSig2() = tmpSig2 + 2
    
    /**
     * Exeption Error: immutableSig3 is immutable variable
     */
    immutableSig3() = 2
   
    /** 
     * As the mutableSig1 and mutableSig2 have been changed. 
     * The immutableSig3 will be re-evaluated.
     */
    println(immutableSig3())
  }
}
```
