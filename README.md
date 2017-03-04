# 水厂矾耗量预测
:blush:本项目使用matlab的神经网络工具箱对水厂的`原水流量`，`原水浊度`，`原水PH`，`单位耗矾流量`的数据进行训练，<br/>
* 自变量为：`原水流量`，`原水浊度`，`原水PH`
* 因变量为：`单位耗矾流量`
* 神经网络参数：<br/>
:point_right:输入神经元个数：1<br/>
:point_right:隐含层数：1<br/>
:point_right:隐含层神经元个数：55<br/>
:point_right:输出神经元个数：1<br/>

神经网络训练好后，将网络参数导出，并使用JS重写此神经网络，使其能够运行在浏览器上。使用HTML做了简单的UI。<br/><br/>
### 成品展示 :metal:
[展示](http://www.buildce.com/demo/sc.html "展示")，代码见`神经网络预测水厂矾耗JS代码.html`文件。 <br/><br/>
### 项目感想 :metal:
感想<br/><br/>
### 关键代码 :metal:
训练好的神经的JS实现代码如下：<br/>
```Javascript
function getResult(){
			var yz = parseFloat(document.getElementById('yz').value);
			var yl = parseFloat(document.getElementById('yl').value);
			var wd = parseFloat(document.getElementById('wd').value);
			var ph = parseFloat(document.getElementById('ph').value);
			var result = document.getElementById('result');

			//网络结构
			var input_num = 4;
			var hidden_num = 55;
			var output_num = 1;
			var x1_step1_xoffset = [0.31,337,-3,6.62];
			var x1_step1_gain = [0.0020006201922596,4.65235292749308e-05,0.0487804878048781,1.18343195266272];
      var x1_step1_ymin = -1;
      // Layer 1
      var b1 =             [-3.811343936962988,2.4644237147744876,4.5291863984869947,3.1106513971356922,-3.7945458440286695,1.7422854671397985,2.3384307221133858,-2.6640886525614946,-1.0593888010848684,2.2369140950635997,4.1027835860578854,2.8807526921841338,1.8279626747426283,1.2403079437241362,1.5444019508542144,-1.7436926935971613,-2.2081205784067879,1.775629322290234,1.8770289138659322,2.3773493825211442,-0.2370507972687638,2.0362253331919056,1.1395377979264703,-2.1759531105538295,-0.54162098670138903,-0.5568707178353719,0.79812663870337819,2.293292308507028,-0.58394257429596874,-1.6863090930062972,1.7574346428924432,-0.36795592486924988,0.80713516096581361,0.044584464350643505,-1.4259283312979494,-0.33646742059087886,1.876175897633414,0.33437463330250894,0.49613550943790152,0.97307700494369731,-1.9053124083285939,2.3797995290073195,2.7153677185243077,-3.0522253451556054,2.9980573077727679,3.3314623448755429,3.2345821419675658,-2.8290511659954802,3.2478853649091262,3.7452601564425079,-4.652061180183316,3.9882509172201082,-3.4349170930208679,3.5010202450931112,5.5433250625624897];
       var IW1_1 = [
            			[2.0127948847207722,1.9718286015115178,-2.339731890993197,1.0592909039616922],
		 				[-1.0406073806332752,0.98353361339528456,1.5054875036514328,-4.4044643165053143],
		 				[-2.3589984018789654,2.4433777083258525 ,1.0274436039466459 ,-1.9465208549466353],
		 				[-2.0540946257692436,2.3423252885772907 ,2.5669452757694864,-0.39438755843322726],
		 				[0.84402385303506777,-3.8060853809433879 ,-3.3644740849769748,0.069151665550821084],
		 				[-2.7162284885111672,-0.66461361007491371 ,-1.9897944223948054,-3.4066437435921699],
		 				[-2.8097254362831445,2.4810372775113834 ,-0.84805512667698957,0.78007491471216839],
		 				[2.9674606479982222,-2.3745539600684129 ,-1.081571729907307, 0.93378374095602612],
		 				[-0.84999091630926149,3.539411119405111, 4.0196344281376195,-1.2076434138458021],
		 				[-1.9911951972444863,-0.10750182078533736, 3.7132034283120223,1.066167786675781],
		 				[1.8310000496222936,4.1752601766541328 ,0.27209184646725521,-1.1338158297532663],
		 				[-3.142394280637776,-2.2641069257662734 ,-0.81461297394636778,1.4814438285684866],
		 				[0.078694232385613322,0.50248206144124674 ,2.0836922587341729,3.7460036397115477],
		 				[-0.62977144589218803,2.2074139658205287, 0.96032865903575515,-2.197850782741309],
		 				[-2.0825999047818202,2.4098124840808932 ,-0.5198519772428739, -2.0056446036445954],
		 				[2.9331229405889387,-2.3034707923536386 ,0.31329728900161136,-0.81136403171272842],
		 				[1.5078157227648936,-1.9356402864174955, 0.88405940413274176,1.6046693553469942],
		 				[-0.72695797076341839,-3.5785841701967738, 1.0257906046387142,0.82873220005277581],
		 				[-2.648614699437391, 0.15588716505572439, 2.223353183420383, -1.3818891066514274],
		 				[1.3201428949301666,-2.3746127181405909 ,-4.238294825607259, 0.59937902698260503],
		 				[0.96390646850349171,-1.8008227332086575 ,1.296560581707985, 3.4161363912713529],
		 				[-2.786709072183323,-2.4325996920297137 ,-0.82534978988852059,-0.0055198330191852203],
		 				[-2.6443178108009087,-2.9403165480123339 ,0.29875372132258948,0.4621237642709019],
		 				[-0.91539166528215576,0.39081204465395658 ,-2.944750503684510, -0.65501527037507945],
		 				[-1.2690209778399428,-2.8372942765814235 ,-1.266928110638151, -0.78677908055122603],
		 				[1.0670569595457688,-0.99954557039743352 ,-1.159405374053238,-2.9319415361737851],
		 				[-0.27946949380284103,-1.6610681703011436 ,0.42001533410716207,2.8412099848398951],
		 				[-0.39955527583838807,2.1101620450099254 ,1.1344600059999834,4.0184290960157591],
		 				[3.6460433062526501,-0.41610656960257886, 2.7980270136469199,-2.7038087140207794],
		 				[-0.85625868433918728 ,-3.2540732742497629 ,1.4218006878162615,-1.0517106552858977],
		 				[3.9058475177299847 ,-1.7880143770728822 ,-1.3927570553314954,0.95194535897603449],
		 				[-4.4737231441073328 ,-0.14442584606668271 ,-1.591370201322579,3.678824697048495],
		 				[2.0901531416257901 ,0.10346242470206485, 6.0576937139916733,2.3376670524087433],
		 				[2.165524546144689 ,3.769376311741429, 1.4231401144848985,-2.5030826364384411],
		 				[-2.6205119131635484, 1.7525074423127927 ,-0.80480566563172506,-2.6909759483303985],
		 				[1.7389452329808506 ,-2.0563206871349666 ,2.5665624986454718,1.0018327597838284],
		 				[3.6201264180745416 ,-3.0504429933524531, -0.89033324927964252,0.47165412439777354],
		 				[1.1440079057257677 ,-0.65962017818572183 ,-3.6344548964826875,2.4960996614138784],
		 				[0.91737386695345879 ,-2.6931892359417819 ,0.0028440788597439808 ,-2.3290831424893716],
		 				[-0.54528688667239067, -0.11239311005520614 ,3.5029778105554983 ,-1.8193786569321155],
		 				[-4.4362840933051979, -0.028050869839226722 ,4.6373339125215658 ,1.2495003198958674],
		 				[3.8179161791318972 ,-1.3844532242482197 ,-1.4089357401923523 ,0.4853857063489489],
		 				[-1.3679174324945769 ,-1.673379709523187, -0.59089055188266193 ,3.2228510316549301],
		 				[-0.41806209252787679, -0.014615409451699365 ,1.1576695084863118 ,3.5520246037205174],
		 				[-0.20632531927120032, -3.7621052314995183 ,-0.10904968512848517 ,0.13096588619733868],
		 				[1.1582535372320097 ,-0.087633536418672206, 0.98241112694546517 ,-3.8383021553561294],
		 				[4.9079552000225686, 0.021982303716018527 ,0.64980262755877938 ,-2.6377199578233865],
		 				[-0.21773211056024538 ,1.5543403123176465 ,4.5400364999380001, 1.1295628765702459],
		 				[2.3128271019793614 ,-0.29598915876293408 ,-1.0619559512916703 ,2.0488861359252599],
		 				[3.3786633957785615 ,2.7314975090656928 ,-1.5755444214838605 ,1.0879395958389939],
		 				[-5.2805235317406787, 0.82948197707579074 ,3.1405150962993345 ,-1.5749455798992735],
		 				[2.5253434758192888 ,3.1976756965663156, 2.3767135421624443 ,1.2798282251263429],
		 				[-2.0751759549318596 ,0.63540306530153789 ,1.257568288441582 ,-3.7790909960578674],
		 				[0.79211388920361525 ,3.6431949619426227 ,-2.4100087393384881, -1.637299983467051],
		 				[0.38710023104525199, -0.39697072313206694 ,0.19020699825082288, 4.613067923533794]
		 				];
			// Layer 2
			var b2 = -0.49812845355432556;
			var LW2_1 = [0.1417846825542759 ,0.74890502971952289 ,-0.18548853773646681 ,-1.0920308621551076 ,0.077221820520713094 ,-1.6574375733870232 ,0.058208957587004964 ,0.49200113867200451 ,0.60721744770812036 ,1.0764584677965932 ,0.28668462544308787 ,1.4119359883094964 ,0.51698834949904815 ,0.60262658747196363 ,0.56566040334552614 ,-0.39084375986169712 ,-1.0646982820765378 ,-0.34731102825007576 ,0.071072567173434936 ,0.64911165302640328 ,0.84591578991203908 ,0.59018294576531594 ,-0.31516447428591088 ,-1.1248332619277492 ,-0.35263699417613259 ,1.3962175572661673 ,-0.76285970248478074 ,0.36733022652719788 ,0.71172940131442386 ,-1.423810880698285, 1.2688533075109198 ,0.80828273029683118 ,-0.10495008930494108 ,-0.87272241152268226 ,0.90215834556969443 ,0.63787147128807109 ,0.75523737182292061 ,0.16337550800245584 ,1.0950768186137876 ,-0.38281283575616509 ,-0.22745543580385741 ,-1.5177291342603154 ,0.86860732653893435 ,0.55507937012328523 ,-0.17923582578458669 ,-1.0369271062713907 ,-0.50124705617029341 ,0.14350648016054121 ,1.7788806536153401 ,-0.54002977851398015 ,-0.23819961989505364 ,0.38481834793513392 ,0.8394156593454144 ,0.31663832002603648 ,-1.8885390085411811];
			// Output 1
			var y1_step1_ymin = -1;
			var y1_step1_gain = 0.000595366943676715;
			var y1_step1_xoffset = 37.01316626;

			//输入层
			var input_layer = [yz,yl,wd,ph];
			//隐含层
			var hidden_layer = [];
			//输出层
			var output_layer = 0.0;

			//输入数据初始化
			for(var i=0;i<input_num;i++){
				input_layer[i] = input_layer[i]-x1_step1_xoffset[i];
				input_layer[i] = input_layer[i]*x1_step1_gain[i];
				input_layer[i] = input_layer[i]+x1_step1_ymin;
			}
			// Layer1
			for(var i=0;i<hidden_num;i++){
				var temp = 0.0;
				for(var j=0;j<input_num;j++){
					temp += IW1_1[i][j]*input_layer[j];
				}
				hidden_layer[i] = tansig(temp+b1[i]);
			}
			// Layer2
			var temp = 0.0;
			for(var i=0;i<hidden_num;i++){
				temp += LW2_1[i]*hidden_layer[i];
			}
			output_layer = temp+b2;
			// Output
			output_layer = output_layer-y1_step1_ymin;
			output_layer = output_layer/y1_step1_gain;
			output_layer = output_layer+y1_step1_xoffset;

			result.innerHTML = output_layer;
		}
		function tansig(x){
			return 2/(1+Math.exp(-2*x))-1;
		}
```
<br/>感谢阅读　:joy:
