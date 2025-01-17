# ⚠️ Notice:
This repository is now moved to a different organisation: [one-to-many version](https://github.com/idlab-discover/pc-webrtc-o2m) and [many-to-many version](https://github.com/idlab-discover/pc-webrtc-m2m).

# WebRTC Point Cloud Streaming

This repository provides an end-to-end pipeline to facilitate one-to-many immersive video streaming with point clouds. To capture the point clouds we employ the Intel Realsense SDK 2.0. However, as raw point clouds require significant bandwidth they are encoded by the Google Draco encoder. One of the main qualities of a good streaming system in the current day-and-age is its ability to adapt to the users's network conditions. For this we implemented a layer-based coding solution ontop of Draco which first samples the source point cloud in three layers, each with a different sampling rate. Additionally, we perform parallel draco encoding on these layers to further reduce latency. 

The WebRTC server and client are implemented in Golang by utilizing the Pion WebRTC package, due to its superior support for the Google Congestion Controller which allows us to estimate each user's bandwidth. By using this estimation the server is able to select the best combination of layers for each user. By decoding the received layers in parallel the receiving client is able to render the point cloud.

In addition we also provide a SFU-based pipeline, more information about this can be found [here](https://github.com/MatthiasDeFre/webrtc-pc-streaming?tab=readme-ov-file#sfu-based-streaming).

## Project Structure

Each component in the architecture is a submodule of this repository and can also be build seperately. The components are designed in a modular fashion which makes it easy to implement your own algorithms if required. As an addition to the compoments we also provide a experiment submodule which can be used to evaluate certain aspects of the pipeline with ease and without the requirement of having specific hardware such as a Intel Realsense camera or Meta Quest 2 HMD. The submodule also has the results from our current evaluations.

## Building

Each submodule has detailed instructions on how to build the corrosponding component. Currently only Windows is supported as a full end-to-end architecture due to the lacking virtual reality support from Unity for Linux-based operating systems. The experiment code on the other hand is supported by both Windows and Ubuntu and allows evaluation of the sampling/encoding, transport and decoding components.


## Dependencies

The provided script (scripts/install_dependancies.ps1) can be used to install all dependancies (except Unity) required for each component. It should be noted that this will take a significant amount of time. For a more fine-tune experience you can also use the install scripts located in each submodule repository. 

## SFU Based Streaming
We also offer a many-to-many SFU-based streaming solution which makes use of the following repositories:

### [SFU Server and Client](https://github.com/MatthiasDeFre/spirit_sfu_june)
This repository contains the code for both the SFU server and WebRTC peer. In addition it also includes a dashboard to test the quality adaptation algorithm. You should not run the WebRTC peer yourself as it gets automatically started when using the Unity application.

### [Unity application](https://github.com/MatthiasDeFre/pc_m2m_unity/)
This repository contains the Unity code, it also contains the dll files required for capturing and encoding as well as the WebRTC peer.

### [WebRTC Connector](https://github.com/MatthiasDeFre/spirit_connector_june/) 
This repository contains the code required to create a dynamic library that is used to connect Unity with the Golang WebRTC peer.

### [Draco Dynamic Library](https://github.com/MatthiasDeFre/spirit_draco_june) 
This repository contains the code required to create a dynamic library that is used to compress the captured or artifical point clouds using a MDC-based approach.

### [Realsense Dynamic Library](https://github.com/MatthiasDeFre/spirit_realsense_june) 
This repository contains the code required to create a dynamic library that is used to capture point clouds using a Realsense camera. Additionaly, it can also create artifical point clouds.
