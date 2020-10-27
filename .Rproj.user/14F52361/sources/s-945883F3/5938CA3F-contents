##' Expand interval dataframe into series
##'
##' Expand dataframe containing an interval into series defined by step size
##'
##' @param data Dataframe
##' @param start Variable that defines starting point of interval / series
##' @param stop Variable that defines end point of interval / series
##' @param step Step size of output series. Default: 1
##' @param timeint Time increment, for dates only; Days, weeks, months, quarters or years. Default: days
##' @param varname Name of series variable. Default: "generate_series"
##' @return Dataframe containing series
##'
##' @export
##'
generateSeries <- function(data, start, stop, step = 1, timeint = "day", varname = "series"){
  date <- inherits(data[,start], "Date")
  if(date){
    step <- paste(step, timeint)
  }
  seq.out <- Map(seq, data[, start], data[, stop], step)
  data.out <- as.data.frame(lapply(data, rep, sapply(seq.out, length)))
  data.out$var <- if(date){as.Date(unlist(seq.out))} else {unlist(seq.out)}
  names(data.out)[which(names(data.out)=="var")] <- varname
  return(data.out[,!names(data.out)%in% c(start, stop)])
}
