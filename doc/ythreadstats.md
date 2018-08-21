\#summary yappi.YThreadStats

= class YThreadStats (v0.92)=

\<font face=\'Consolas\'\>

\<b\>\<i\>class\</i\>\</b\> yappi.\<b\>YThreadStats\</b\>

\<b\>get()\</b\> This method retrieves the current thread stats.
yappi.get~threadstats~() is actually just a wrapper for this function.
YThreadStats holds the stat items as a list of [YThreadStat]{.underline}
object.

A [YThreadStat]{.underline} object holds the following information:

  -- -------------- -- --------------------------------------------------------------------------------------------------------------------------------------------------------------- --
     **Key**           **Description**                                                                                                                                                 
     id                thread id given by the OS                                                                                                                                       
     name              class name of the current thread object which is derived from threading.Thread class                                                                            
     ttot              total time spent in the last executed function. See \[<https://code.google.com/p/yappi/wiki/ClockTypes_v082> Clock Types\] to interpret this value correctly.   
     sched~count~      number of times this thread is scheduled.                                                                                                                       
  -- -------------- -- --------------------------------------------------------------------------------------------------------------------------------------------------------------- --

\<b\>print~all~(out=sys.stdout, columns={0:(\"name\",13), 1:(\"tid\",
15), 2:(\"ttot\", 8), 3:(\"scnt\", 10)})\</b\> This method prints the
current profile stats to \<i\>\"out\"\</i\> which is
\<i\>\"stdout\"\</i\> by default. \<i\>columns\</i\> dict specifies the
formatting of the printable columns. Every column is formatted according
to the parameters of this dict\'s item.

An example item: 0:(\"name\",13) means that the first column in position
\'0\' has title name and its size is 13. Yappi will allocate 13
characters for this column when printing.

\<b\>sort(sort~type~, sort~order~=\"desc\")\</b\> This method sorts the
current profile stats according to the \<i\>\"sort~type~\"\</i\> param.

The following are the valid \<i\>\"sort~type~\"\</i\> params:

  -- ----------------- --
     **Sort Types**    
     name              
     id                
     totaltime/ttot    
     schedcount/scnt   
  -- ----------------- --

The following are the valid \<i\>\"sort~order~\"\</i\> params:

  -- ----------------- --
     **Sort Orders**   
     descending/desc   
     ascending/asc     
  -- ----------------- --

\<b\>clear()\</b\> Clears the retrieved stats. Note that this only
clears the current object\'s stat list. You need to explicitly call
[yappi.clear~stats~()]{.underline} to clear the current profile stats.

\<b\>empty()\</b\> Returns a boolean indicating whether we have any
stats available or not.

\</font\>