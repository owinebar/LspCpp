# LspCpp
依赖: boost 和 rapidjson
使用:
 1.使用VS2017打开,还原nuget包
 2.编译debug 或者release版本就行
 
 这个工程是用来支持JCIDE ,配合jdt.ls和我们写的scp使用的.
 等有空在详细写写.
 例子:
```cpp
	sct_initialize::request request;
	request.params.processId = processId;
	auto  eventFuture = std::make_shared< Condition< LspMessage > >();
	sct->sendRequest(request, [&](std::unique_ptr<LspMessage> msg)
		{
			eventFuture->notify(std::move(msg));
			return true;
		});
	auto msg = eventFuture->wait(100000);
	if (!msg)
	{
		return false;
	}
	auto  result = dynamic_cast<sct_initialize::response*>(msg.get());
	if (result)
	{
		sctServerCapabilities  _lsServerCapabilities;
		_lsServerCapabilities.swap(result->result.capabilities);
		//Notify_InitializedNotification::notify _notify;
		//sct->sendNotification(_notify);
		return true;
	}
	else
	{
		auto error = reinterpret_cast<Rsp_Error*>(msg.get());
		log->log(Log::Level::SEVERE, error->error.ToString());
		return false;
	}

```
