# Write-ahead logging   

In computer science, write-ahead logging is a family of techniques for providing atomicity and durability in database systems.    
The changes are first recorded in the log, which must be written to stable storage, before the changes are written to the database.    

在计算机领域，WAL（Write-ahead logging，预写式日志）是数据库系统提供原子性和持久化的一系列技术。    

在使用WAL的系统中，所有的修改都先被写入到日志中，然后再被应用到系统状态中。<b> 通常包含redo和undo两部分信息。</b>    



