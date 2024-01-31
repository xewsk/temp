# If the operation name of the first span is matching, this segment should be ignored
#  ant path match style
#  /path/?   Match any single character
#  /path/*   Match any number of characters
#  /path/**  Match any number of characters and support multilevel directories
#  Multiple path comma separation, like trace.ignore_path=/eureka/**,/consul/**
trace.ignore_path=${SW_AGENT_TRACE_IGNORE_PATH:/eureka/**}
