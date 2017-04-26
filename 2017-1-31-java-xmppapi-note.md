---
layout: post
title: "smack java接口(xmpp)笔记"
category: "project"
keywords: ["project", "smack","android","xmpp"]
description: "smack java接口（xmpp）"
tags: ["project", "smack","android","xmpp"]
---
### 文档实在是看不下去，就从各种博客，教程上整理了这些 。-_^
http://blog.csdn.net/wangyi_lin/article/details/6953606
## 1.连接

```
ConnectionConfiguration connConfig = new ConnectionConfiguration("123.207.174.226", 5222);			
XMPPConnection con = new XMPPConnection(connConfig);
connConfig.setSendPresence(false);//设置状态为离线
con.connect();
```

## 2.断开

```
con.disconnect();
```

## 3.登陆

```
con.login("user1","123456");
```

## 4.发消息

```
ChatManager cm =con.getChatManager();
Chat newchat = cm.createChat("user2@123.207.174.226", null);
String msg = "qwewerwer";
newchat.sendMessage(msg);
```

## 5.注册用户

```
public static boolean createAccount(XMPPConnection connection,String regUserName,String regUserPwd)
{
  try {
    connection.getAccountManager().createAccount(regUserName, regUserPwd);
    return true;
  } catch (Exception e) {
    return false;
  }
}
```

## 6.删除当前用户

```
public static boolean deleteAccount(XMPPConnection connection)
{
  try {
    connection.getAccountManager().deleteAccount();
    return true;
  } catch (Exception e) {
    return false;
  }
}
```

## 7.监听消息1

```
class TaxiChatManagerListener implements ChatManagerListener {

    public void chatCreated(Chat chat, boolean arg1) {
        chat.addMessageListener(new MessageListener() {
            public void processMessage(Chat arg0, Message msg) {

                //发送消息用户
                msg.getFrom();
                //消息内容
                String from = msg.getFrom();
                String body = msg.getBody();

                String[] user_name = from.split("@");
                System.out.println(user_name[0]+" "+body);
            }
        });
    }
}
TaxiChatManagerListener chatManagerListener = new TaxiChatManagerListener();
con.getChatManager().addChatListener(chatManagerListener);
//监听消息2
ChatManager manager = con.getChatManager();  
manager.addChatListener(new ChatManagerListener() {  
	  public void chatCreated(Chat chat, boolean arg1) {  
	      chat.addMessageListener(new MessageListener() {  
	          public void processMessage(Chat arg0, Message msg) {  
	              //若是聊天窗口已存在，将消息转往目前窗口   
	              //若是窗口不存在，开新的窗口并注册   
	              //发送消息用户
	              msg.getFrom();
	              //消息内容
	              String from = msg.getFrom();
	              String body = msg.getBody();

	              String[] user_name = from.split("@");
	              System.out.println(user_name[0]+" : "+body);
	          }     
	      });  
	  }  
});
//
MessageListener msgListener = new MessageListener()
{
    public void processMessage(Chat chat, Message message)
    {
          if (message != null && message.getBody() != null)
          {
              System.out.println("收到消息:" + message.getBody());
              // 可以在这进行针对这个用户消息的处理，但是这里我没做操作，看后边聊天窗口的控制
          }
    }
};
Chat chat = Client.getConnection().getChatManager().createChat(userName, msgListener);
```

## 8.获取好友列表

```
Roster roster = XMPP_data.connection.getRoster();
Collection<RosterGroup> entriesGroup = roster.getGroups();
for(RosterGroup group: entriesGroup){
    Collection<RosterEntry> entries = group.getEntries();
    Log.d("group", group.getName());
    for (RosterEntry entry : entries) {
        System.out.println("name:"+entry.getName());
    }
}
```

## 9.获取离线消息

```
//需要先将状态设置为离线（connConfig.setSendPresence(false);//设置状态为离线）
OfflineMessageManager offlineManager = new OfflineMessageManager(con);  
try
{  
    Iterator<org.jivesoftware.smack.packet.Message> it = offlineManager.getMessages();  
    System.out.println(offlineManager.supportsFlexibleRetrieval());  
    System.out.println("离线消息数量: " + offlineManager.getMessageCount());  
    Map<String,ArrayList<Message>> offlineMsgs = new HashMap<String,ArrayList<Message>>();  

        while (it.hasNext()) {  
            org.jivesoftware.smack.packet.Message message = it.next();  
            System.out.println("收到离线消息, Received from 【" + message.getFrom() + "】 message: " + message.getBody());  
            String fromUser = message.getFrom().split("/")[0];  

            if(offlineMsgs.containsKey(fromUser))  
            {  
                offlineMsgs.get(fromUser).add(message);  
            }else{  
                ArrayList<Message> temp = new ArrayList<Message>();  
                temp.add(message);  
                offlineMsgs.put(fromUser, temp);  
            }  
        }  

        offlineManager.deleteMessages(); //删除离线消息
} catch (Exception e) {  
    e.printStackTrace();  
}
Presence presence = new Presence(Presence.Type.available);
con.sendPacket(presence);//设置状态为在线
```



## 10.搜索好友

```
String searchService = "search."+con.getServiceName();
try{
    UserSearchManager search = new UserSearchManager(con);
    Form searchForm = search.getSearchForm(searchService);
    Form answerForm = searchForm.createAnswerForm();
    answerForm.setAnswer("Username", true);
    answerForm.setAnswer("search","user");
    ReportedData data = search.getSearchResults(answerForm,searchService);

    Iterator<Row> it = data.getRows();
    Row row=null;
    String ansS="";
    while(it.hasNext()){
        row=it.next();
        ansS=row.getValues("Username").next().toString();
        System.out.println(ansS);
    }
    //Toast.makeText(this,ansS, Toast.LENGTH_SHORT).show();
}catch(Exception e){
    System.out.print(e);
}
```
在android平台使用会报错，这是需要添加一行
```
ProviderManager.getInstance().addIQProvider("query", "jabber:iq:search", new UserSearch.Provider());
```

```
ProviderManager.getInstance().addIQProvider("query", "jabber:iq:search", new UserSearch.Provider());
String searchService = "search."+XMPP_data.connection.getServiceName();
Log.d("server",XMPP_data.connection.getServiceName());
try{
    UserSearchManager search = new UserSearchManager(XMPP_data.connection);
    Form searchForm = search.getSearchForm(searchService);

    Form answerForm = searchForm.createAnswerForm();

    answerForm.setAnswer("Username", true);
    answerForm.setAnswer("search",username.trim());
    ReportedData data = search.getSearchResults(answerForm,searchService);
    Iterator<ReportedData.Row> it = data.getRows();
    ReportedData.Row row=null;
    String ansS="";

    while(it.hasNext()){
        row=it.next();
        ansS=row.getValues("Username").next().toString();
        Log.d("user",ansS);
        Message message = handler.obtainMessage();
        Bundle b = new Bundle();
        b.putString("user",ansS);
        message.setData(b);
        handler.sendMessage(message);
    }
    //Toast.makeText(this,ansS, Toast.LENGTH_SHORT).show();
}catch(Exception e){
    Log.d("search error:",e.toString());
}
```
然后在XMPPConnection初始化时添加
```
configure(ProviderManager.getInstance());
```
configure函数如下：
```
public static void configure(ProviderManager pm)
    {

        // Private Data Storage
        pm.addIQProvider("query", "jabber:iq:private", new PrivateDataManager.PrivateDataIQProvider());

        // Time
        try
        {
            pm.addIQProvider("query", "jabber:iq:time", Class.forName("org.jivesoftware.smackx.packet.Time"));
        }
        catch (ClassNotFoundException e)
        {
            Log.w("TestClient", "Can't load class for org.jivesoftware.smackx.packet.Time");
        }

        // Roster Exchange
        pm.addExtensionProvider("x", "jabber:x:roster", new RosterExchangeProvider());

        // Message Events
        pm.addExtensionProvider("x", "jabber:x:event", new MessageEventProvider());

        // Chat State
        pm.addExtensionProvider("active", "http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        pm.addExtensionProvider("composing", "http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        pm.addExtensionProvider("paused", "http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        pm.addExtensionProvider("inactive", "http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        pm.addExtensionProvider("gone", "http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());

        // XHTML
        pm.addExtensionProvider("html", "http://jabber.org/protocol/xhtml-im", new XHTMLExtensionProvider());

        // Group Chat Invitations
        pm.addExtensionProvider("x", "jabber:x:conference", new GroupChatInvitation.Provider());

        // Service Discovery # Items
        pm.addIQProvider("query", "http://jabber.org/protocol/disco#items", new DiscoverItemsProvider());

        // Service Discovery # Info
        pm.addIQProvider("query", "http://jabber.org/protocol/disco#info", new DiscoverInfoProvider());

        // Data Forms
        pm.addExtensionProvider("x", "jabber:x:data", new DataFormProvider());

        // MUC User
        pm.addExtensionProvider("x", "http://jabber.org/protocol/muc#user", new MUCUserProvider());

        // MUC Admin
        pm.addIQProvider("query", "http://jabber.org/protocol/muc#admin", new MUCAdminProvider());

        // MUC Owner
        pm.addIQProvider("query", "http://jabber.org/protocol/muc#owner", new MUCOwnerProvider());

        // Delayed Delivery
        pm.addExtensionProvider("x", "jabber:x:delay", new DelayInformationProvider());

        // Version
        try
        {
            pm.addIQProvider("query", "jabber:iq:version", Class.forName("org.jivesoftware.smackx.packet.Version"));
        }
        catch (ClassNotFoundException e)
        {
            // Not sure what's happening here.
        }

        // VCard
        pm.addIQProvider("vCard", "vcard-temp", new VCardProvider());

        // Offline Message Requests
        pm.addIQProvider("offline", "http://jabber.org/protocol/offline", new OfflineMessageRequest.Provider());

        // Offline Message Indicator
        pm.addExtensionProvider("offline", "http://jabber.org/protocol/offline", new OfflineMessageInfo.Provider());

        // Last Activity
        pm.addIQProvider("query", "jabber:iq:last", new LastActivity.Provider());

        // User Search
        pm.addIQProvider("query", "jabber:iq:search", new UserSearch.Provider());

        // SharedGroupsInfo
        pm.addIQProvider("sharedgroup", "http://www.jivesoftware.org/protocol/sharedgroup", new SharedGroupsInfo.Provider());

        // JEP-33: Extended Stanza Addressing
        pm.addExtensionProvider("addresses", "http://jabber.org/protocol/address", new MultipleAddressesProvider());

        // FileTransfer
        pm.addIQProvider("si", "http://jabber.org/protocol/si", new StreamInitiationProvider());

        pm.addIQProvider("query", "http://jabber.org/protocol/bytestreams", new BytestreamsProvider());

        // Privacy
        pm.addIQProvider("query", "jabber:iq:privacy", new PrivacyProvider());
        pm.addIQProvider("command", "http://jabber.org/protocol/commands", new AdHocCommandDataProvider());
        pm.addExtensionProvider("malformed-action", "http://jabber.org/protocol/commands", new AdHocCommandDataProvider.MalformedActionError());
        pm.addExtensionProvider("bad-locale", "http://jabber.org/protocol/commands", new AdHocCommandDataProvider.BadLocaleError());
        pm.addExtensionProvider("bad-payload", "http://jabber.org/protocol/commands", new AdHocCommandDataProvider.BadPayloadError());
        pm.addExtensionProvider("bad-sessionid", "http://jabber.org/protocol/commands", new AdHocCommandDataProvider.BadSessionIDError());
        pm.addExtensionProvider("session-expired", "http://jabber.org/protocol/commands", new AdHocCommandDataProvider.SessionExpiredError());
    }
```
## 11.添加好友
```
try {
    Roster roster1 = con.getRoster();
    roster1.createEntry("user4@123.207.174.226", "nickName", new String[]{"test1"});//发送添加好友请求，并将该jid用户加入到自己的roster中
    System.out.println("add friend ok");
}catch(Exception e){
    System.out.print("error add ");
}

Presence p = new Presence(Presence.Type.subscribe);
p.setTo("user2@123.207.174.226");
con.sendPacket(p);
```

## 12.添加好友监听
```
//条件过滤器
PacketFilter filter = new AndFilter(new PacketTypeFilter(Presence.class));
//packet监听器
PacketListener listener = new PacketListener() {

    @Override
    public void processPacket(Packet packet) {
      System.out.println("PresenceService-"+packet.toXML());
      if(packet instanceof Presence){
            Presence presence = (Presence)packet;
            String from = presence.getFrom();//发送方
            String to = presence.getTo();//接收方
            if (presence.getType().equals(Presence.Type.subscribe)) {
                System.out.println("收到添加请求！");
            } else if (presence.getType().equals(Presence.Type.subscribed)) {
                //发送广播传递response字符串
                String response = "恭喜，对方同意添加好友！";
            } else if (presence.getType().equals(Presence.Type.unsubscribe)) {
                //发送广播传递response字符串
                String response = "抱歉，对方拒绝添加好友，将你从好友列表移除！";
            } else if (presence.getType().equals(Presence.Type.unsubscribed)){

            } else if (presence.getType().equals(Presence.Type.unavailable)) {
                System.out.println("好友下线！");
            } else {
                System.out.println("好友上线！");
            }
      }
    }
};
con.addPacketListener(listener, filter); //添加监听
```

## 14.群聊
创建群聊
public class ChatManager(){
  public static void createroom(){
    MultiUserChatManager manager = mew MultiUserChatManager.getInstance
  }
}
public void createroom(){

}
加入群聊

搜索群

## 13.文件收发(未成功)

```
public static void sendFile(XMPPConnection connection,String user, File file) throws XMPPException, InterruptedException {  
        File f1 =new File("c:\\abc\\1.txt");
        System.out.println("发送文件开始"+file.getName());  
        FileTransferManager transfer = new FileTransferManager(con);  
        System.out.println("发送文件给: "+user+Client.getServiceNameWithPre());  
        OutgoingFileTransfer out = transfer.createOutgoingFileTransfer(user+Client.getServiceNameWithPre()+"/Smack");//  

        out.sendFile(file, file.getName());  

        System.out.println("//////////");  
        System.out.println(out.getStatus());  
        System.out.println(out.getProgress());  
        System.out.println(out.isDone());  

        System.out.println("//////////");  

        System.out.println("发送文件结束");  
}
FileTransferManager transfer = new FileTransferManager(connection);  
transfer.addFileTransferListener(new RecFileTransferListener());  

public class RecFileTransferListener implements FileTransferListener {  

    public String getFileType(String fileFullName)  
    {  
        if(fileFullName.contains("."))  
        {  
            return "."+fileFullName.split("//.")[1];  
        }else{  
            return fileFullName;  
        }  

    }  

    @Override  
    public void fileTransferRequest(FileTransferRequest request) {  
        System.out.println("接收文件开始.....");  
        final IncomingFileTransfer inTransfer = request.accept();  
        final String fileName = request.getFileName();  
        long length = request.getFileSize();   
        final String fromUser = request.getRequestor().split("/")[0];  
        System.out.println("文件大小:"+length + "  "+request.getRequestor());  
        System.out.println(""+request.getMimeType());  
        try {   

            JFileChooser chooser = new JFileChooser();   
            chooser.setCurrentDirectory(new File("."));   

            int result = chooser.showOpenDialog(null);  

            if(result==JFileChooser.APPROVE_OPTION)  
            {  
                final File file = chooser.getSelectedFile();  
                System.out.println(file.getAbsolutePath());  
                    new Thread(){  
                        public void run()  
                        {  
                        try {  

                            System.out.println("接受文件: " + fileName);  
                            inTransfer  
                                    .recieveFile(new File(file  
                                            .getAbsolutePath()  
                                            + getFileType(fileName)));  

                            Message message = new Message();  
                            message.setFrom(fromUser);  
                            message.setProperty("REC_SIGN", "SUCCESS");  
                            message.setBody("["+fromUser+"]发送文件: "+fileName+"/r/n"+"存储位置: "+file.getAbsolutePath()+ getFileType(fileName));  
                            if (Client.isChatExist(fromUser)) {  
                                Client.getChatRoom(fromUser).messageReceiveHandler(  
                                        message);  
                            } else {  
                                ChatFrameThread cft = new ChatFrameThread(  
                                        fromUser, message);  
                                cft.start();  

                            }  
                        } catch (Exception e2) {  
                            e2.printStackTrace();  
                        }  
                        }  
                    }.start();  
            }else{  

                System.out.println("拒绝接受文件: "+fileName);  

                request.reject();  
                Message message = new Message();  
                message.setFrom(fromUser);  
                message.setBody("拒绝"+fromUser+"发送文件: "+fileName);  
                message.setProperty("REC_SIGN", "REJECT");  
                if (Client.isChatExist(fromUser)) {  
                    Client.getChatRoom(fromUser)  
                            .messageReceiveHandler(message);  
                } else {  
                    ChatFrameThread cft = new ChatFrameThread(  
                            fromUser, message);  
                    cft.start();  
                }  
            }  





            /* InputStream in = inTransfer.recieveFile();

             String fileName = "r"+inTransfer.getFileName();

             OutputStream out = new FileOutputStream(new File("d:/receive/"+fileName));
             byte[] b = new byte[512];
             while(in.read(b) != -1)
             {
                 out.write(b);
                 out.flush();
             }

             in.close();
             out.close();*/  
        } catch (Exception e) {  
            e.printStackTrace();  
        }  

        System.out.println("接收文件结束.....");  

    }  

}    


FileTransferManager transfer = new FileTransferManager(connection);  
transfer.addFileTransferListener(new RecFileTransferListener());

public class RecFileTransferListener implements FileTransferListener {
  public void fileTransferRequest(final FileTransferRequest request) {
        IncomingFileTransfer inTransfer = request.accept();
        File file = new File(XmppConnection.SAVE_PATH + "/"+ request.getFileName());  //接收文件路径
        inTransfer.recieveFile(file);
  }
}
```

## 获取Openfire离线消息乱码问题解决方案  

1）Mysql数据库系统编码问题，修改mysql配置文件my.cnf，将数据库系统默认字符集设置成utf8，再重启Mysql：

```
[mysqld]
character-set-server=utf8
[mysql]
default-character-set=utf8
```

2）还是老生常谈，建库，建表，建字段时也指定编码是utf8。

```
create database openfire default character set utf8 default collate utf8_general_ci;
```

3）修改Openfire配置文件，conf下的openfire.xml的serverURL改成这个
jdbc:mysql://127.0.0.1:3306/openfire?rewriteBatchedStatements=true&amp;amp;useUnicode=true&amp;amp;characterEncoding=utf-8
或者在设置时，将默认的JDBC地址后再加上&useUnicode=true&characterEncoding=utf-8

4）在Android客户端Java在获取离线信息字符串时用UTF-8来编码即可。
