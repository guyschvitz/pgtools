##' Remove locks from postgres tables
##'
##' Function finds and removes all locks from tables that match search terms,
##' helps to avoid sending queries that hang without returning results
##'
##' @param conn \code{DBI} connection object
##' @param table regular expression for table name (case insensitive)
##'
##' @export data.frame with table and schema names
##'

pgRemoveLocks <- function(conn, table){
  locks <- dbGetQuery(conn, statement =  paste0("select pid from pg_locks l join
  pg_class t on l.relation = t.oid where t.relkind = 'r' and t.relname LIKE '%",
                                               table_name, "%';"))
  if(nrow(locks)==0){
    print("No locks found")
  } else {
    lapply(1:nrow(locks),
           function(x){dbSendQuery(con,
                                   statement = paste0("SELECT pg_cancel_backend(",
                                                           locks[x,], ")"))})
    print(nrow(locks),  "locks removed")
  }
}
