1、map的keySet()方法会获取map key的视图，这个视图如果改变，会影响map的数据。  
比如remove，会remove掉map对应的数据，values()方法remove掉对应的值后，也会影响map.  
执行add方法，会抛出UnsupportedOperationException异常
2、

