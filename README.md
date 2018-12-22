# Notes
TestNotes
####     创建线程只有一种方式，那就是创建Thread类，而实现线程的执行单元有两种方式，  

    一种是重写Thread类的run方法，另一种是实现runnable接口，并把Runnable实例当做参数传入到Thread构造方法中去。
    
### 模板设计模式在Thread的应用

    线程的真正的执行逻辑是在run方法中，通常我们会把run方法称为线程的执行单元，用start方法启动线程
    Thread的run和start就是一个比较典型的模板设计模式，父类编写算法结构代码，子类实现逻辑细节
    
    public class Template {
 
    public Template() {
    }
 
    public final void start(){
        System.out.println("stat方法启动！");
        run();
        System.out.println("start方法结束！");
    }
 
    public void run(){}
 
    public static void main(String[] args) {
 
        Template template = new Template(){
            @Override
            public void run() {
                System.out.println("run方法开始运行！");
            }
        };
 
        template.start();
    }
    
    
    运行结果：
    
    stat方法启动！
    run方法开始运行！
    start方法结束！
    
### 策略模式
    
    package com.jack.strategy;
    /** 
     * 策略接口，主要是规范或者让结构程序知道如何进行调用 
     */
    public interface CalcStrategy {
    	int calc(int x, int y);
    }
    
    package com.jack.strategy;
     
    /** 
     * 程序的结构，里面约束了整个程序的框架和执行的大概流程，但并未涉及到业务层面的东西 
     * 只是将一个数据如何流入如何流出做了规范，只是提供了一个默认的逻辑实现 
     * @author Administrator 
     */ 
    class Caculator {
    	private int x = 0;
    	private int y = 0;
    	private CalcStrategy strategy = null;
    	
    	public Caculator(int x,int y){
    		this.x = x;
    		this.y = y;
    	}
    	
    	public Caculator(int x,int y,CalcStrategy strategy){
    		this(x,y);
    		this.strategy = strategy;
    	}
    	
    	public int calc(int x, int y) {
    		return x+y;
    	}
    	
    	/** 
    	 
    	 * 只需关注接口，并且将接口用到的入参传递进去即可，并不关心到底具体是要如何进行业务封装 
    	 
    	 * @return 
    	 
    	 */
    	
    	public int result(){
    		if(null != strategy){
    			return strategy.calc(x, y);      
    		}
    		
    		return calc(x,y);
    	}
    	
    	public void start(){
    		System.out.println(result());
    	}
    	
    }
     
    class AddSrategy implements CalcStrategy{
     
    	public int calc(int x, int y) {
    		// TODO Auto-generated method stub
    		return x + y;
    	}
    	
    }
     
    class SubStrategty implements CalcStrategy{
     
    	public int calc(int x, int y) {
    		// TODO Auto-generated method stub
    		return x - y;
    	}
    	
    }
    
    
    package com.jack.strategy;
     
    public class StrategyTest {
    	public static void main(String args[] ) {
    		//没有任何策略时的结果
    		Caculator c1 = new Caculator(1,1);
    		c1.start();
    		//传入减法策略的结果
    		Caculator c2 = new Caculator(10,30, new SubStrategty());
    		c2.start();
    		
    		//看到这里就可以看到策略模式强大了，算法可以随意设置，系统的结构并不会发生任何变化
    		Caculator c3  = new Caculator(2,3,new CalcStrategy(){
    			public int calc(int x, int y) {
    				// TODO Auto-generated method stub
    				return x+10/(y+1)*2;
    			}
    		});
    		
    		c3.start();
    	}
    	
    }
    
    -----------------------------------------------------------------------------------------------------
    
    3.Thread  ：template desigher   strategy designer 对照
    package com.jack.thread1;
     
    public class ThreadTest  {
     
     
    	public static void main(String args[]){
    		new Thread(new  Runnable(){
     
    			public void run() {
    				System.out.println(Thread.currentThread().getName());
    			}}
    		).start();
    		
    	}
    }
    
