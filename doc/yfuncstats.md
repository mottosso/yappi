\#summary yappi.YFuncStats

= class YFuncStats (v0.92)=

\<font face=\'Consolas\'\>

\<b\>\<i\>class\</i\>\</b\> yappi.\<b\>YFuncStats\</b\>

\<b\>get()\</b\> This method retrieves the current profiling stats.
yappi.get~funcstats~() is actually just a wrapper for this function.
YFuncStats holds the stat items as a list of [YFuncStat]{.underline}
object.

A [YFuncStat]{.underline} object holds the following information:

  -- ------------- -- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ --
     **Key**          **Description**                                                                                                                                                                
     index            A unique number for the stat                                                                                                                                                   
     module           Module name of the executed function                                                                                                                                           
     lineno           Line number of the executed function                                                                                                                                           
     name             Name of the executed function                                                                                                                                                  
     full~name~       module:lineno name - unique full name of the executed function                                                                                                                 
     ncall            number of times the executed function is called.                                                                                                                               
     nactualcall      number of times the executed function is called, excluding the recursive calls.                                                                                                
     builtin          boolean flag showing if the executed function is a builtin                                                                                                                     
     ttot             total time spent in the executed function. See \[<https://code.google.com/p/yappi/wiki/ClockTypes_v082> Clock Types\] to interpret this value correctly.                       
     tsub             total time spent in the executed function, excluding subcalls. See \[<https://code.google.com/p/yappi/wiki/ClockTypes_v082> Clock Types\] to interpret this value correctly.   
     tavg             per-call average total time spent in the executed function. See \[<https://code.google.com/p/yappi/wiki/ClockTypes_v082> Clock Types\] to interpret this value correctly.      
     children         list of functions called from the executed function. See \[<https://code.google.com/p/yappi/wiki/YChildFuncStats_v092> YChildFuncStats\] object                                
  -- ------------- -- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ --

\<b\>add(path, type=\"ystat\")\</b\> This method loads the saved profile
stats stored in file \<i\>\"path\"\</i\>. \<i\>\"type\"\</i\> indicates
the type of the saved profile stats.

The following are the load formats currently available:

  -- ------------ --
     **Format**   
     ystat        
  -- ------------ --

\<b\>save(path, type=\"ystat\")\</b\> This method saves the current
profile stats to \<i\>\"path\"\</i\>. \<i\>\"type\"\</i\> indicates the
target type that the profile stats will be saved in. Currently only
loading from \<i\>\"ystat\"\</i\> format is possible.
\<i\>\"ystat\"\</i\> is the current yappi internal format.

The following are the save formats currently available:

  -- ------------------------------------------------------------------------------------------------------ --
     **Format**                                                                                             
     ystat                                                                                                  
     \[<http://docs.python.org/3.3/library/profile.html?highlight=pstat#pstats.Stats.print_stats> pstat\]   
     \[<http://kcachegrind.sourceforge.net/html/CallgrindFormat.html> callgrind\]                           
  -- ------------------------------------------------------------------------------------------------------ --

\<b\>print~all~(out=sys.stdout, columns={0:(\"name\",36), 1:(\"ncall\",
5), 2:(\"tsub\", 8), 3:(\"ttot\", 8), 4:(\"tavg\",8)})\</b\> This method
prints the current profile stats to \<i\>\"out\"\</i\> which is
\<i\>\"stdout\"\</i\> by default. \<i\>columns\</i\> dict specifies the
formatting of the printable columns. Every column is formatted according
to the parameters of this dict\'s item.

An example item: 0:(\"name\",36) means that the first column in position
\'0\' has title name and its size is 36. Yappi will allocate 36
characters