##常用代码
获取鼠标点击，然后自动寻路
   
	//nma = GetComponent<NavMeshAgent>();
	void Update () {
        if (Input.GetMouseButtonDown(0))
        {
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            RaycastHit hit;
            if (Physics.Raycast(ray, out hit))
            {
                nma.SetDestination(hit.point);
            }
        }	
	}

从prefab创建实例

	//注意，TeddyBear.prefab需要放在resources目录里，resources的上层目录名字无限制
	void Start () {
        Object prefab = Resources.Load("TeddyBear"); 
        for (int i = 0; i < 100; i++)
        {
            Instantiate(prefab, new Vector3(0, 0, 0), Quaternion.identity);
		}

	}

响应手机返回键或Home键退出消息
    
	void Update ()
    {
        //点击手机返回键关闭应用程序
        if (Input.GetKeyDown(KeyCode.Escape) || Input.GetKeyDown(KeyCode.Home) )
        {
            Application.Quit();
        }
    }
 
##Mecanim动画
使用步骤

1. 模型的Rig标签下选择Humanoid,生成Avatar
2. 创建AnimationController，把动画文件的每个动作拖动到里面作为State，设定参数来控制动作转换
3. 代码中用类似 `animator.SetFloat("Speed",v)`，来修改状态

##Android上查看profiler
adb forward tcp:54999 localabstract:Unity-这里加你的包名
