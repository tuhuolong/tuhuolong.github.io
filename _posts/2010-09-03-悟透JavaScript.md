---
layout: post
title: "悟透JavaScript"
date: 2010-09-03 17:33:00 
comments: true
categories: [javascript]
tags: [javascript]
description: "悟透JavaScript"
keywords: javascript
---


 
  
   
    http://edlongren.blogbus.com/logs/20492154.html
   
  
 
 
 
 
  编程世界里只存在两种基本元素，一个是数据，一个是代码。编程世界就是在数据和代码千丝万缕的纠缠中呈现出无限的生机和活力。
 
 
  数据天生就是文静的，总想保持自己固有的本色；而代码却天生活泼，总想改变这个世界。
 
 
  你看，数据代码间的关系与物质能量间的关系有着惊人的相似。数据也是有惯性的，如果没有代码来施加外力，她总保持自己原来的状态。而代码就象能量，他存在的唯一目的，就是要努力改变数据原来的状态。在代码改变数据的同时，也会因为数据的抗拒而反过来影响或改变代码原有的趋势。甚至在某些情况下，数据可以转变为代码，而代码却又有可能被转变为数据，或许还存在一个类似E=MC2形式的数码转换方程呢。然而，就是在数据和代码间这种即矛盾又统一的运转中，总能体现出计算机世界的规律，这些规律正是我们编写的程序逻辑。
 
 
  不过，由于不同程序员有着不同的世界观，这些数据和代码看起来也就不尽相同。于是，不同世界观的程序员们运用各自的方法论，推动着编程世界的进化和发展。
 
 
  众所周知，当今最流行的编程思想莫过于面向对象编程的思想。为什么面向对象的思想能迅速风靡编程世界呢？因为面向对象的思想首次把数据和代码结合成统一体，并以一个简单的对象概念呈现给编程者。这一下子就将原来那些杂乱的算法与子程序，以及纠缠不清的复杂数据结构，划分成清晰而有序的对象结构，从而理清了数据与代码在我们心中那团乱麻般的结。我们又可以有一个更清晰的思维，在另一个思想高度上去探索更加浩瀚的编程世界了。
 
 
  在五祖弘忍讲授完《对象真经》之后的一天，他对众弟子们说：“经已讲完，想必尔等应该有所感悟，请各自写个偈子来看”。大弟子神秀是被大家公认为悟性最高的师兄，他的偈子写道：“身是对象树，心如类般明。朝朝勤拂拭，莫让惹尘埃！”。此偈一出，立即引起师兄弟们的轰动，大家都说写得太好了。只有火头僧慧能看后，轻轻地叹了口气，又随手在墙上写道：“对象本无根，类型亦无形。本来无一物，何处惹尘埃？”。然后摇了摇头，扬长而去。大家看了慧能的偈子都说：“写的什么乱七八糟的啊，看不懂”。师父弘忍看了神秀的诗偈也点头称赞，再看慧能的诗偈之后默然摇头。就在当天夜里，弘忍却悄悄把慧能叫到自己的禅房，将珍藏多年的软件真经传授于他，然后让他趁着月色连夜逃走...
 
 
  后来，慧能果然不负师父厚望，在南方开创了禅宗另一个广阔的天空。而慧能当年带走的软件真经中就有一本是《JavaScript真经》！
 
 
  回归简单
 
 
  要理解JavaScript，你得首先放下对象和类的概念，回到数据和代码的本原。前面说过，编程世界只有数据和代码两种基本元素，而这两种元素又有着纠缠不清的关系。JavaScript就是把数据和代码都简化到最原始的程度。
 
 
  JavaScript中的数据很简洁的。简单数据只有 undefined, null, boolean, number和string这五种，而复杂数据只有一种，即object。这就好比中国古典的朴素唯物思想，把世界最基本的元素归为金木水火土，其他复杂的物质都是由这五种基本元素组成。
 
 
  JavaScript中的代码只体现为一种形式，就是function。
 
 
  注意：以上单词都是小写的，不要和Number, String, Object, Function等JavaScript内置函数混淆了。要知道，JavaScript语言是区分大小写的呀!
 
 
  任何一个JavaScript的标识、常量、变量和参数都只是unfined, null, bool, number, string, object 和 function类型中的一种，也就typeof返回值表明的类型。除此之外没有其他类型了。
 
 
  先说说简单数据类型吧。
 
 
  undefined:   代表一切未知的事物，啥都没有，无法想象，代码也就更无法去处理了。
  
  注意：typeof(undefined) 返回也是 undefined。
  
  可以将undefined赋值给任何变量或属性，但并不意味了清除了该变量，反而会因此多了一个属性。
 
 
  null:            有那么一个概念，但没有东西。无中似有，有中还无。虽难以想象，但已经可以用代码来处理了。
  
  注意：typeof(null)返回object，但null并非object，具有null值的变量也并非object。
 
 
  boolean:      是就是，非就非，没有疑义。对就对，错就错，绝对明确。既能被代码处理，也可以控制代码的流程。
 
 
  number:      线性的事物，大小和次序分明，多而不乱。便于代码进行批量处理，也控制代码的迭代和循环等。
  
  注意：typeof(NaN)和typeof(Infinity)都返回number 。
  
  NaN参与任何数值计算的结构都是NaN，而且 NaN != NaN 。
  
  Infinity / Infinity = NaN 。
 
 
  string:         面向人类的理性事物，而不是机器信号。人机信息沟通，代码据此理解人的意图等等，都靠它了。
 
 
  简单类型都不是对象，JavaScript没有将对象化的能力赋予这些简单类型。直接被赋予简单类型常量值的标识符、变量和参数都不是一个对象。
 
 
  所谓“对象化”，就是可以将数据和代码组织成复杂结构的能力。JavaScript中只有object类型和function类型提供了对象化的能力。
 
 
  没有类
 
 
  object就是对象的类型。在JavaScript中不管多么复杂的数据和代码，都可以组织成object形式的对象。
 
 
  但JavaScript却没有 “类”的概念！
 
 
  对于许多面向对象的程序员来说，这恐怕是JavaScript中最难以理解的地方。是啊，几乎任何讲面向对象的书中，第一个要讲的就是“类”的概念，这可是面向对象的支柱。这突然没有了“类”，我们就象一下子没了精神支柱，感到六神无主。看来，要放下对象和类，达到“对象本无根，类型亦无形”的境界确实是件不容易的事情啊。
 
 
  这样，我们先来看一段JavaScript程序：
  
  var life = {};
  
  for(life.age = 1; life.age <= 3; life.age++)
  
  {
  
  switch(life.age)
  
  {
  
  case 1: life.body = "卵细胞";
  
  life.say = function(){alert(this.age+this.body)};
  
  break;
  
  case 2: life.tail = "尾巴";
  
  life.gill = "腮";
  
  life.body = "蝌蚪";
  
  life.say = function(){alert(this.age+this.body+"-"+this.tail+","+this.gill)};
  
  break;
  
  case 3: delete life.tail;
  
  delete life.gill;
  
  life.legs = "四条腿";
  
  life.lung = "肺";
  
  life.body = "青蛙";
  
  life.say = function(){alert(this.age+this.body+"-"+this.legs+","+this.lung)};
  
  break;
  
  };
  
  life.say();
  
  };
  
  这段JavaScript程序一开始产生了一个生命对象life，life诞生时只是一个光溜溜的对象，没有任何属性和方法。在第一次生命过程中，它有了一个身体属性body，并有了一个say方法，看起来是一个“卵细胞”。在第二次生命过程中，它又长出了“尾巴”和“腮”，有了tail和gill属性，显然它是一个“蝌蚪”。在第三次生命过程中，它的tail和gill属性消失了，但又长出了“四条腿”和“肺”，有了legs和lung属性，从而最终变成了“青蛙”。如果，你的想像力丰富的话，或许还能让它变成英俊的“王子”，娶个美丽的“公主”什么的。不过，在看完这段程序之后，请你思考一个问题：
 
 
  我们一定需要类吗？
 
 
  还记得儿时那个“小蝌蚪找妈妈”的童话吗？也许就在昨天晚，你的孩子刚好是在这个美丽的童话中进入梦乡的吧。可爱的小蝌蚪也就是在其自身类型不断演化过程中，逐渐变成了和妈妈一样的“类”，从而找到了自己的妈妈。这个童话故事中蕴含的编程哲理就是：对象的“类”是从无到有，又不断演化，最终又消失于无形之中的...
 
 
  “类”，的确可以帮助我们理解复杂的现实世界，这纷乱的现实世界也的确需要进行分类。但如果我们的思想被“类”束缚住了，“类”也就变成了“累”。想象一下，如果一个生命对象开始的时就被规定了固定的“类”，那么它还能演化吗？蝌蚪还能变成青蛙吗？还可以给孩子们讲小蝌蚪找妈妈的故事吗？
 
 
  所以，JavaScript中没有“类”，类已化于无形，与对象融为一体。正是由于放下了“类”这个概念，JavaScript的对象才有了其他编程语言所没有的活力。
 
 
  如果，此时你的内心深处开始有所感悟，那么你已经逐渐开始理解JavaScript的禅机了。
 
 
  函数的魔力
 
 
  接下来，我们再讨论一下JavaScript函数的魔力吧。
 
 
  JavaScript的代码就只有function一种形式，function就是函数的类型。也许其他编程语言还有procedure或 method等代码概念，但在JavaScript里只有function一种形式。当我们写下一个函数的时候，只不过是建立了一个function类型的实体而已。请看下面的程序：
  
  function myfunc()
  
  {
  
  alert("hello");
  
  };
  
  
  alert(typeof(myfunc));
  
  这个代码运行之后可以看到typeof(myfunc)返回的是function。以上的函数写法我们称之为“定义式”的，如果我们将其改写成下面的“变量式”的，就更容易理解了：
  
  var myfunc = function ()
  
  {
  
  alert("hello");
  
  };
  
  
  alert(typeof(myfunc));
  
  这里明确定义了一个变量myfunc，它的初始值被赋予了一个function的实体。因此，typeof(myfunc)返回的也是function。其实，这两种函数的写法是等价的，除了一点细微差别，其内部实现完全相同。也就是说，我们写的这些JavaScript函数只是一个命了名的变量而已，其变量类型即为function，变量的值就是我们编写的函数代码体。
 
 
  聪明的你或许立即会进一步的追问：既然函数只是变量，那么变量就可以被随意赋值并用到任意地方啰？
 
 
  我们来看看下面的代码：
  
  var myfunc = function ()
  
  {
  
  alert("hello");
  
  };
  
  myfunc(); //第一次调用myfunc，输出hello
  
  
  myfunc = function ()
  
  {
  
  alert("yeah");
  
  };
  
  myfunc(); //第二次调用myfunc，将输出yeah
  
  这个程序运行的结果告诉我们：答案是肯定的！在第一次调用函数之后，函数变量又被赋予了新的函数代码体，使得第二次调用该函数时，出现了不同的输出。
 
 
  好了，我们又来把上面的代码改成第一种定义式的函数形式：
  
  function myfunc ()
  
  {
  
  alert("hello");
  
  };
  
  myfunc(); //这里调用myfunc，输出yeah而不是hello
  
  
  function myfunc ()
  
  {
  
  alert("yeah");
  
  };
  
  myfunc(); //这里调用myfunc，当然输出yeah
  
  按理说，两个签名完全相同的函数，在其他编程语言中应该是非法的。但在JavaScript中，这没错。不过，程序运行之后却发现一个奇怪的现象：两次调用都只是最后那个函数里输出的值！显然第一个函数没有起到任何作用。这又是为什么呢？
 
 
  原来，JavaScript执行引擎并非一行一行地分析和执行程序，而是一段一段地分析执行的。而且，在同一段程序的分析执行中，定义式的函数语句会被提取出来优先执行。函数定义执行完之后，才会按顺序执行其他语句代码。也就是说，在第一次调用myfunc之前，第一个函数语句定义的代码逻辑，已被第二个函数定义语句覆盖了。所以，两次都调用都是执行最后一个函数逻辑了。
 
 
  如果把这个JavaScript代码分成两段，例如将它们写在一个html中，并用<script/>;标签将其分成这样的两块：
  
  <script>;
  
  function myfunc ()
  
  {
  
  alert("hello");
  
  };
  
  myfunc(); //这里调用myfunc，输出hello
  
  </script>;
 
 
  <script>;
  
  function myfunc ()
  
  {
  
  alert("yeah");
  
  };
  
  myfunc(); //这里调用myfunc，输出yeah
  
  </script>;
  
  这时，输出才是各自按顺序来的，也证明了JavaScript的确是一段段地执行的。
 
 
  一段代码中的定义式函数语句会优先执行，这似乎有点象静态语言的编译概念。所以，这一特征也被有些人称为：JavaScript的“预编译”。
 
 
  大多数情况下，我们也没有必要去纠缠这些细节问题。只要你记住一点：JavaScript里的代码也是一种数据，同样可以被任意赋值和修改的，而它的值就是代码的逻辑。只是，与一般数据不同的是，函数是可以被调用执行的。
 
 
  不过，如果JavaScript函数仅仅只有这点道行的话，这与C++的函数指针，DELPHI的方法指针，C#的委托相比，又有啥稀奇嘛！然而，JavaScript函数的神奇之处还体现在另外两个方面：一是函数function类型本身也具有对象化的能力，二是函数function与对象 object超然的结合能力。
 
 
  奇妙的对象
 
 
  先来说说函数的对象化能力。
 
 
  任何一个函数都可以为其动态地添加或去除属性，这些属性可以是简单类型，可以是对象，也可以是其他函数。也就是说，函数具有对象的全部特征，你完全可以把函数当对象来用。其实，函数就是对象，只不过比一般的对象多了一个括号“()”操作符，这个操作符用来执行函数的逻辑。即，函数本身还可以被调用，一般对象却不可以被调用，除此之外完全相同。请看下面的代码：
  
  function Sing()
  
  {
  
  with(arguments.callee)
  
  alert(author + "：" + poem);
  
  };
  
  Sing.author = "李白";
  
  Sing.poem = "汉家秦地月，流影照明妃。一上玉关道，天涯去不归";
  
  Sing();
  
  Sing.author = "李战";
  
  Sing.poem = "日出汉家天，月落阴山前。女儿琵琶怨，已唱三千年";
  
  Sing();
  
  在这段代码中，Sing函数被定义后，又给Sing函数动态地增加了author和poem属性。将author和poem属性设为不同的作者和诗句，在调用Sing()时就能显示出不同的结果。这个示例用一种诗情画意的方式，让我们理解了JavaScript函数就是对象的本质，也感受到了JavaScript语言的优美。
 
 
  好了，以上的讲述，我们应该算理解了function类型的东西都是和object类型一样的东西，这种东西被我们称为“对象”。我们的确可以这样去看待这些“对象”，因为它们既有“属性”也有“方法”嘛。但下面的代码又会让我们产生新的疑惑：
  
  var anObject = {};  //一个对象
  
  anObject.aProperty = "Property of object";  //对象的一个属性
  
  anObject.aMethod = function(){alert("Method of object")}; //对象的一个方法
  
  //主要看下面：
  
  alert(anObject["aProperty"]);   //可以将对象当数组以属性名作为下标来访问属性
  
  anObject["aMethod"]();          //可以将对象当数组以方法名作为下标来调用方法
  
  for( var s in anObject)           //遍历对象的所有属性和方法进行迭代化处理
  
  alert(s + " is a " + typeof(anObject[s]));
  
  同样对于function类型的对象也是一样：
  
  var aFunction = function() {};  //一个函数
  
  aFunction.aProperty = "Property of function";  //函数的一个属性
  
  aFunction.aMethod = function(){alert("Method of function")}; //函数的一个方法
  
  //主要看下面：
  
  alert(aFunction["aProperty"]);   //可以将函数当数组以属性名作为下标来访问属性
  
  aFunction["aMethod"]();          //可以将函数当数组以方法名作为下标来调用方法
  
  for( var s in aFunction)           //遍历函数的所有属性和方法进行迭代化处理
  
  alert(s + " is a " + typeof(aFunction[s]));
  
  是的，对象和函数可以象数组一样，用属性名或方法名作为下标来访问并处理。那么，它到底应该算是数组呢，还是算对象？
 
 
  我们知道，数组应该算是线性数据结构，线性数据结构一般有一定的规律，适合进行统一的批量迭代操作等，有点像波。而对象是离散数据结构，适合描述分散的和个性化的东西，有点像粒子。因此，我们也可以这样问：JavaScript里的对象到底是波还是粒子？
 
 
  如果存在对象量子论，那么答案一定是：波粒二象性！
 
 
  因此，JavaScript里的函数和对象既有对象的特征也有数组的特征。这里的数组被称为“字典”，一种可以任意伸缩的名称值对儿的集合。其实， object和function的内部实现就是一个字典结构，但这种字典结构却通过严谨而精巧的语法表现出了丰富的外观。正如量子力学在一些地方用粒子来解释和处理问题，而在另一些地方却用波来解释和处理问题。你也可以在需要的时候，自由选择用对象还是数组来解释和处理问题。只要善于把握JavaScript的这些奇妙特性，就可以编写出很多简洁而强大的代码来。
 
 
  放下对象
 
 
  我们再来看看function与object的超然结合吧。
 
 
  在面向对象的编程世界里，数据与代码的有机结合就构成了对象的概念。自从有了对象，编程世界就被划分成两部分，一个是对象内的世界，一个是对象外的世界。对象天生具有自私的一面，外面的世界未经允许是不可访问对象内部的。对象也有大方的一面，它对外提供属性和方法，也为他人服务。不过，在这里我们要谈到一个有趣的问题，就是“对象的自我意识”。
 
 
  什么？没听错吧？对象有自我意识？
 
 
  可能对许多程序员来说，这的确是第一次听说。不过，请君看看C++、C#和Java的this，DELPHI的self，还有VB的me，或许你会恍然大悟！当然，也可能只是说句“不过如此”而已。
 
 
  然而，就在对象将世界划分为内外两部分的同时，对象的“自我”也就随之产生。“自我意识”是生命的最基本特征！正是由于对象这种强大的生命力，才使得编程世界充满无限的生机和活力。
 
 
  但对象的“自我意识”在带给我们快乐的同时也带来了痛苦和烦恼。我们给对象赋予了太多欲望，总希望它们能做更多的事情。然而，对象的自私使得它们互相争抢系统资源，对象的自负让对象变得复杂和臃肿，对象的自欺也往往带来挥之不去的错误和异常。我们为什么会有这么多的痛苦和烦恼呢？
 
 
  为此，有一个人，在对象树下，整整想了九九八十一天，终于悟出了生命的痛苦来自于欲望，但究其欲望的根源是来自于自我意识。于是他放下了“自我”，在对象树下成了佛，从此他开始普度众生，传播真经。他的名字就叫释迦摩尼，而《JavaScript真经》正是他所传经书中的一本。
 
 
  JavaScript中也有this，但这个this却与C++、C#或Java等语言的this不同。一般编程语言的this就是对象自己，而 JavaScript的this却并不一定！this可能是我，也可能是你，可能是他，反正是我中有你，你中有我，这就不能用原来的那个“自我”来理解 JavaScript这个this的含义了。为此，我们必须首先放下原来对象的那个“自我”。
 
 
  我们来看下面的代码：
  
  function WhoAmI()       //定义一个函数WhoAmI
  
  {
  
  alert("I'm " + this.name + " of " + typeof(this));
  
  };
  
  
  WhoAmI();   //此时是this当前这段代码的全局对象，在浏览器中就是window对象，其name属性为空字符串。输出：I'm of object
 
 
  var BillGates = {name: "Bill Gates"};
  
  BillGates.WhoAmI = WhoAmI;  //将函数WhoAmI作为BillGates的方法。
  
  BillGates.WhoAmI();         //此时的this是BillGates。输出：I'm Bill Gates of object
  
  
  var SteveJobs = {name: "Steve Jobs"};
  
  SteveJobs.WhoAmI = WhoAmI;  //将函数WhoAmI作为SteveJobs的方法。
  
  SteveJobs.WhoAmI();         //此时的this是SteveJobs。输出：I'm Steve Jobs of object
 
 
  WhoAmI.call(BillGates);     //直接将BillGates作为this，调用WhoAmI。输出：I'm Bill Gates of object
  
  WhoAmI.call(SteveJobs);     //直接将SteveJobs作为this，调用WhoAmI。输出：I'm Steve Jobs of object
  
  
  BillGates.WhoAmI.call(SteveJobs);   //将SteveJobs作为this，却调用BillGates的WhoAmI方法。输出：I'm Steve Jobs of object
  
  SteveJobs.WhoAmI.call(BillGates);   //将BillGates作为this，却调用SteveJobs的WhoAmI方法。输出：I'm Bill Gates of object
 
 
  WhoAmI.WhoAmI = WhoAmI;     //将WhoAmI函数设置为自身的方法。
  
  WhoAmI.name = "WhoAmI";
  
  WhoAmI.WhoAmI();            //此时的this是WhoAmI函数自己。输出：I'm WhoAmI of function
  
  
  ({name: "nobody", WhoAmI: WhoAmI}).WhoAmI();    //临时创建一个匿名对象并设置属性后调用WhoAmI方法。输出：I'm nobody of object
  
  从上面的代码可以看出，同一个函数可以从不同的角度来调用，this并不一定是函数本身所属的对象。this只是在任意对象和function元素结合时的一个概念，是种结合比起一般对象语言的默认结合更加灵活，显得更加超然和洒脱。
 
 
  在JavaScript函数中，你只能把this看成当前要服务的“这个”对象。this是一个特殊的内置参数，根据this参数，您可以访问到“这个”对象的属性和方法，但却不能给this参数赋值。在一般对象语言中，方法体代码中的this可以省略的，成员默认都首先是“自己”的。但JavaScript却不同，由于不存在“自我”，当访问“这个”对象时，this不可省略！
 
 
  JavaScript提供了传递this参数的多种形式和手段，其中，象BillGates.WhoAmI()和SteveJobs.WhoAmI()这种形式，是传递this参数最正规的形式，此时的this就是函数所属的对象本身。而大多数情况下，我们也几乎很少去采用那些借花仙佛的调用形式。但只我们要明白JavaScript的这个“自我”与其他编程语言的“自我”是不同的，这是一个放下了的“自我”，这就是JavaScript特有的世界观。
 
 
  对象素描
 
 
  已经说了许多了许多话题了，但有一个很基本的问题我们忘了讨论，那就是：怎样建立对象？
 
 
  在前面的示例中，我们已经涉及到了对象的建立了。我们使用了一种被称为JavaScript Object Notation(缩写JSON)的形式，翻译为中文就是“JavaScript对象表示法”。
 
 
  JSON为创建对象提供了非常简单的方法。例如，
  
  创建一个没有任何属性的对象：
  
  var o = {};
  
  创建一个对象并设置属性及初始值：
  
  var person = {name: "Angel", age: 18, married: false};
  
  创建一个对象并设置属性和方法：
  
  var speaker = {text: "Hello World", say: function(){alert(this.text)}};
  
  创建一个更复杂的对象，嵌套其他对象和对象数组等：
  
  var company =
  
  {
  
  name: "Microsoft",
  
  product: "softwares",
  
  chairman: {name: "Bill Gates", age: 53, Married: true},
  
  employees: [{name: "Angel", age: 26, Married: false}, {name: "Hanson", age: 32, Marred: true}],
  
  readme: function() {document.write(this.name + " product " + this.product);}
  
  };
  
  JSON的形式就是用大括“{}”号包括起来的项目列表，每一个项目间并用逗号“,”分隔，而项目就是用冒号“:”分隔的属性名和属性值。这是典型的字典表示形式，也再次表明了 JavaScript里的对象就是字典结构。不管多么复杂的对象，都可以被一句JSON代码来创建并赋值。
 
 
  其实，JSON就是JavaScript对象最好的序列化形式，它比XML更简洁也更省空间。对象可以作为一个JSON形式的字符串，在网络间自由传递和交换信息。而当需要将这个JSON字符串变成一个JavaScript对象时，只需要使用eval函数这个强大的数码转换引擎，就立即能得到一个JavaScript内存对象。正是由于JSON的这种简单朴素的天生丽质，才使得她在AJAX舞台上成为璀璨夺目的明星。
 
 
  JavaScript就是这样，把面向对象那些看似复杂的东西，用及其简洁的形式表达出来。卸下对象浮华的浓妆，还对象一个眉目清晰！
 
 
  构造对象
 
 
  好了，接下我们来讨论一下对象的另一种创建方法。
 
 
  除JSON外，在JavaScript中我们可以使用new操作符结合一个函数的形式来创建对象。例如：
  
  function MyFunc() {};         //定义一个空函数
  
  var anObj = new MyFunc();  //使用new操作符，借助MyFun函数，就创建了一个对象
  
  JavaScript的这种创建对象的方式可真有意思，如何去理解这种写法呢？
 
 
  其实，可以把上面的代码改写成这种等价形式：
  
  function MyFunc(){};
  
  var anObj = {};     //创建一个对象
  
  MyFunc.call(anObj); //将anObj对象作为this指针调用MyFunc函数
  
  我们就可以这样理解，JavaScript先用new操作符创建了一个对象，紧接着就将这个对象作为this参数调用了后面的函数。其实，JavaScript内部就是这么做的，而且任何函数都可以被这样调用！但从 “anObj = new MyFunc()” 这种形式，我们又看到一个熟悉的身影，C++和C#不就是这样创建对象的吗？原来，条条大路通灵山，殊途同归啊！
 
 
  君看到此处也许会想，我们为什么不可以把这个MyFunc当作构造函数呢？恭喜你，答对了！JavaScript也是这么想的！请看下面的代码：
  
  function Person(name)   //带参数的构造函数
  
  {
  
  this.name = name;   //将参数值赋给给this对象的属性
  
  this.SayHello = function() {alert("Hello, I'm " + this.name);};   //给this对象定义一个SayHello方法。
  
  };
 
 
  function Employee(name, salary)     //子构造函数
  
  {
  
  Person.call(this, name);        //将this传给父构造函数
  
  this.salary = salary;       //设置一个this的salary属性
  
  this.ShowMeTheMoney = function() {alert(this.name + " $" + this.salary);};  //添加ShowMeTheMoney方法。
  
  };
  
  
  var BillGates = new Person("Bill Gates");   //用Person构造函数创建BillGates对象
  
  var SteveJobs = new Employee("Steve Jobs", 1234);   //用Empolyee构造函数创建SteveJobs对象
 
 
  BillGates.SayHello();   //显示：I'm Bill Gates
  
  SteveJobs.SayHello();   //显示：I'm Steve Jobs
  
  SteveJobs.ShowMeTheMoney();   //显示：Steve Jobs $1234
 
 
  alert(BillGates.constructor == Person);  //显示：true
  
  alert(SteveJobs.constructor == Employee);  //显示：true
  
  
  alert(BillGates.SayHello == SteveJobs.SayHello); //显示：false
  
  这段代码表明，函数不但可以当作构造函数，而且还可以带参数，还可以为对象添加成员和方法。其中的第9行，Employee构造函数又将自己接收的this作为参数调用Person构造函数，这就是相当于调用基类的构造函数。第21、22行还表明这样一个意思：BillGates是由Person构造的，而SteveJobs是由Employee构造的。对象内置的constructor属性还指明了构造对象所用的具体函数！
 
 
  其实，如果你愿意把函数当作“类”的话，她就是“类”，因为她本来就有“类”的那些特征。难道不是吗？她生出的儿子各个都有相同的特征，而且构造函数也与类同名嘛！
 
 
  但要注意的是，用构造函数操作this对象创建出来的每一个对象，不但具有各自的
 
 
  本文来自CSDN博客，转载请标明出处：
  
   http://blog.csdn.net/chief1985/archive/2008/09/21/2957338.aspx
  
 


