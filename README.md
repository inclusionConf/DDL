This supplementary material provides additional information to the manuscript, including deepfake generation pipeline, manipulated examples, experimental details, and deepfake methods.

## Deepfake Image Generation
### Single-Face Scene Manipulation:
As shown in Fig. 1(a), 
(1) Face Parsing Stage: We utilize the dlib model to extract facial landmark points and generate random facial region masks based on these landmarks. 
(2) Face Generation Stage: Face images and their corresponding masks are fed into a visual generative model, which synthesizes locally tampered images under the guidance of either the masks or textual prompts (required by certain models). We employ DeepSeek-V3 to generate the relevant textual prompts. In this study, we focus on models operating within the Facial Editing (FE) paradigm, as detailed in Table below.

### Multi-face Scene Manipulation:
As depicted in Fig. 1(b),
(1) Face Preprocessing Stage: MTCNN is employed for single-face detection, followed by mask segmentation of each detected face region using a U-Net model.
(2) Face Generation Stage: Randomly selected single-face images are input into the visual generation model to obtain forged images. Similarly, we employ DeepSeek-V3 to generate textual prompts for generative models that require text conditioning.
(3) Face Fusion Stage: The forged faces and their masks are jointly fed into a fusion model with the original multi-face image to generate locally forged multi-face images. 
The fusion method integrates a handcrafted Poisson blending method and a diffusion-based fusion model.
The generation models in this scenario span four forgery modes: Face Swapping (FS), Full-Face Synthesis (FFS), Face Editing (FE), and Face Reenactment (FR).
In addition, benefiting from the decoupled nature of the multi-face forgery process, we introduce an Hybrid Facial Forgery (HFF) Mode (as shown in Figure below), wherein different deepfake techniques are applied to different faces within the same multi-face image.

<div align="center">
  <img src="https://github.com/inclusionConf/DDL/blob/main/generation_pipeline_img_1.jpg">
</div>
Fig.1 Image generation pipeline. The red boxes denote forged images. Italic text indicates generation processing operations. The Generation Model refers to different visual generation models. The Face Blending denotes the fusion model for integrating the forged face with the real background. The LLM synthesizes face-related prompts.

## Deepfake Audio-Video Generation
### Audio-Visual Local Manipulation:
As shown in Fig. 2(a),
(1) Audio Preprocessing Stage: We use FFmpeg to separate video and audio streams, apply a Denoiser to extract clean speech and background noise components, and employ Whisper for speech recognition to obtain textual transcripts of the human voice. Subsequently, we employ DeepSeek-V3 to perform local text manipulation, including replacement, deletion, and insertion operations.
(2) Audio Forgery Stage: The manipulated text is then fed into an audio generation model to synthesize forged audio, whose length is normalized before being merged with the background noise to form the final forged audio sample.
(3) Video Forgery Stage: We employ audio-driven face generation models to synthesize forged face videos conditioned on fake audio. Furthermore, to diversify the types of local audio-visual forgery, we incorporate non-audio-driven video generation models. In this setting, we adopt a disentangled pipeline: original audio and video streams are first separated, then processed by independent audio and video forgery models, respectively, before being recombined into a final forged sequence.
(4) Audio-Visual Merging Stage: According to replacement, deletion, and insertion editing strategies, the forged audio and forged video are combined to create three types of forged audio-visual content: Fake Audio and Fake Visual, Fake Audio and Real Visual, and Real Audio and Fake Visual.
Additionally, we introduce an Audio-Visual Asynchronous Manipulation (AVAM) mode (as shown in Figure below) in which forged audio and video are not synchronized, thereby further mimicking real-world scenarios.

### Audio-Visual Full Synthesis:
As shown in Fig. 2(b),
(1) Prompt Generation Stage: We employ DeepSeek-V3 to generate face-related prompts, such as textual descriptions of expressions and actions like speaking, singing, and laughing.
(2) Video Synthesis Stage: The generated prompt text is input into a Text2Video generation model to obtain the corresponding video content; alternatively, the prompt text, along with a facial image, is provided to an Image2Video model to generate a fully synthesized video featuring the given face.
(3) Audio Synthesis Stage: The synthesized video is then fed into a Video2Audio generation model to produce the corresponding audio track.
(4) Audio-Visual Merging Stage: The resulting facial video and synthesized audio are combined to form a complete, fully synthesized audio-visual sample.
Furthermore, end-to-end audio-visual synthesis models, such as Kling 2.1 and Wan 2.1, are also employed to further improve synthesis quality. This novel forgery paradigm differs significantly from traditional audio-driven or video-driven deepfake techniques, substantially reducing artifacts that may be introduced by multi-frame compositing.

<div align="center">
  <img src="https://github.com/inclusionConf/DDL/blob/main/generation_pipeline_av_1.pdf.png">
</div>
Fig.2 Audio-Visual generation pipeline. The LLM manipulates speech transcripts (a) or synthesizes face prompts (b). Generation Model denotes visual/audio generation models. Red waveforms/faces indicate manipulated audio/video. <Replace/Delete/Insert> in green boxes denote local manipulations. Italicized Whisper/Denoiser denote transcript generation/noise separation; other italics indicate preprocessing. Dashed lines represent optional inputs.

## Manipulated Examples
We primarily demonstrate examples of three innovative forgery patterns and different blending methods in image datasets.

### Forgery Mode Examples
In this section, we present innovative forgery modes as follows:

**Hybrid Facial Forgery (HFF) Mode**: As illustrated in Fig. below, compared to conventional forgery mode, HFF applies two distinct forgery techniques (A and B) to manipulate different faces within a multi-person image. This approach increases the complexity of forgery content, reflecting the diverse forgery landscape in real-world scenarios.

**Audio-Visual Asynchronous Manipulation (AVAM) Mode**: As shown in Fig. below, AVAM selects different temporal segments to modify audio content and video facial regions separately. This method also aligns with the multifaceted nature of real-world forgeries. Moreover, in audio-visual modalities, we incorporate the HFF forgery pattern by applying varied manipulation techniques across different temporal segments.

**Audio-Visual Full Synthesis (AVFS) Mode**: As presented in Fig. below, AVFS generates complete audio-visual content using text or single images. In the era of rapidly advancing artificial intelligence and generative technologies, such forgery patterns are expected to proliferate in real-world contexts.

### Blending Method Examples
As illustrated in Fig. below, (a) depicts Poisson fusion, a method commonly employed in previous research. (b) represents the state-of-the-art diffusion-based fusion technique, which, with the advancement of Artificial Intelligence and Generative Computing (AIGC) technologies, gradually emerges as the predominant fusion approach in real-world scenarios. Our DDL deliberately adopts this method to simulate contemporary trends and challenges in image manipulation.

<div align="center">
  <img src="https://github.com/inclusionConf/DDL/blob/main/supp_forgery_mode_1.pdf.png">
</div>
Fig.3 Examples of Hybrid Facial Forgery (HFF) Mode.

<div align="center">
  <img src="https://github.com/inclusionConf/DDL/blob/main/supp_forgery_mode_2.pdf.png">
</div>
Fig.4 Examples of Audio-Visual Asynchronous Manipulation (AVAM) Mode. The specific audio-visual sample demonstrations are stored in Example_AVAM_Mode folder within the same supplementary material archive.

<div align="center">
  <img src="https://github.com/inclusionConf/DDL/blob/main/supp_forgery_mode_3.pdf.png">
</div>
Fig.5 Examples of Audio-Visual Full Synthesis (AVFS) Mode. The specific audio-visual sample demonstrations are stored in Example_AVFS_Mode folder within the same supplementary material archive.

<div align="center">
  <img src="https://github.com/inclusionConf/DDL/blob/main/supp_forgery_mode_4.pdf.png">
</div>
Fig.6 Examples of Blending Methods.

## Experimental Details
In this section, we provide detailed descriptions of the baseline models, training procedures, and the use of unseen test sets.

### Baseline Models
Image task: We select the face forgery localization method MSCCNet and the conventional image manipulation localization models SparseViT and Mesorch. MSCCNet has a classification head that can simultaneously output image classification and forgery localization results, while SparseViT and Mesorch lack the classification head and can only output forgery localization results. In this study, we take the average of the forgery masks predicted by SparseViT and Mesorch as the image classification results, following the setting of MVSS-Net.

Audio-Visual task: We choose audio-visual localization models BA-TFD, BA-TFD+, and UMMAFormer, all of which lack classification heads. We use the maximum score of the predicted proposals as the image classification result.

### Training Details
We refer to the open-source versions of the aforementioned models.

<table>
    <tr>
        <th>Type</th>
        <th>Method</th>
        <th>Sub-Type</th>
        <th>Venue</th>
    </tr>
    <tr>
        <td rowspan="8">FS</td>
        <td>FSGAN</td>
        <td>GAN based</td>
        <td>ICCV 2019</td>
    </tr>
    <tr>
        <td>SimSwap</td>
        <td>GAN based</td>
        <td>ACM MM 2020</td>
    </tr>
    <tr>
        <td>InSwapper</td>
        <td>GAN based</td>
        <td>Unknown</td>
    </tr>
      <tr>
        <td>BlendFace</td>
        <td>GAN based</td>
        <td>ICCV 2023</td>
    </tr>
    <tr>
        <td>MobileFaceSwap</td>
        <td>GAN based</td>
        <td>AAAI 2022</td>
    </tr>
    <tr>
        <td>DCFace</td>
        <td>Diffusion based</td>
        <td>ECCV 2022</td>
    </tr>
    <tr>
        <td>UniFace</td>
        <td>GAN based</td>
        <td>ECCV 2022</td>
    </tr>
  <tr>
        <td>FaceDancer</td>
        <td>GAN based</td>
        <td>WACV 2023</td>
    </tr>
    <tr>
        <td rowspan="20">FFS</td>
        <td>StyleGAN2</td>
        <td>GAN based</td>
        <td>CVPR 2020</td>
    </tr>
    <tr>
        <td>StyleGAN3</td>
        <td>GAN based</td>
        <td>Arxiv 2023</td>
    </tr>
    <tr>
        <td>InstantID</td>
        <td>Diffusion based</td>
        <td>Arxiv 2024</td>
    </tr>
    <tr>
        <td>IP-Adapter</td>
        <td>Diffusion based</td>
        <td>Arxiv 2023</td>
    </tr>
    <tr>
        <td>SDFlow</td>
        <td>Flow based</td>
        <td>ICASSP 2024</td>
    </tr>
  <tr>
        <td>StyleFlow</td>
        <td>Flow based</td>
        <td>TOG 2021</td>
    </tr>
  <tr>
        <td>RDDM</td>
        <td>Diffusion based</td>
        <td>CVPR 2024</td>
    </tr>
  <tr>
        <td>ICLight</td>
        <td>Diffusion based</td>
        <td>ICLR 2025</td>
    </tr>
  <tr>
        <td>ArcFace</td>
        <td>AR based</td>
        <td>CVPR 2019</td>
    </tr>
    <tr>
        <td>Hailuoai</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>Kling1.6</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>PixVerse</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>Vidu2.0</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>Vivago</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>Wan2.1</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>Jimeng2.0</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>Pika1.5</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>Jimeng3.0</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>Kling2.1</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
  <tr>
        <td>MAGI-1</td>
        <td>Commercial Software</td>
        <td>2025</td>
    </tr>
    <tr>
        <td rowspan="23">FE</td>
        <td>Pluralistic</td>
        <td>GAN based</td>
        <td>CVPR 2019</td>
    </tr>
    <tr>
        <td>FaceInpainting</td>
        <td>GAN based</td>
        <td>TCSVT 2023</td>
    </tr>
    <tr>
        <td>HRFAE</td>
        <td>GAN based</td>
        <td>ICPR 2021</td>
    </tr>
    <tr>
        <td>Repaint</td>
        <td>Diffusion based</td>
        <td>CVPR 2022</td>
    </tr>
    <tr>
        <td>Paint by Example</td>
        <td>Diffusion based</td>
        <td>CVPR 2023</td>
    </tr>
    <tr>
        <td>SDXL-v1_Inpainting</td>
        <td>Diffusion based</td>
        <td>Arxiv 2023</td>
    </tr>
    <tr>
        <td>Realistic_Vision_V5.0</td>
        <td>Diffusion based</td>
        <td>Arxiv 2023</td>
    </tr>
    <tr>
        <td>SD-v2_inpainting</td>
        <td>Diffusion based</td>
        <td>Arxiv 2023</td>
    </tr>
    <tr>
        <td>SD-v1.5_Inpainting</td>
        <td>Diffusion based</td>
        <td>Arxiv 2023</td>
    </tr>
    <tr>
        <td>Inpaint Anything-SD</td>
        <td>Diffusion based</td>
        <td>Arxiv 2023</td>
    </tr>
    <tr>
        <td>StyleRes</td>
        <td>Diffusion based</td>
        <td>CVPR 2023</td>
    </tr>
    <tr>
        <td>UltraEdit</td>
        <td>Diffusion based</td>
        <td>NIPS 2024</td>
    </tr>
  <tr>
        <td>EXE-GAN</td>
        <td>GAN based</td>
        <td>Neurocomputing 2025</td>
    </tr>
  <tr>
        <td>MI-GAN</td>
        <td>GAN based</td>
        <td>ICCV 2023</td>
    </tr>
  <tr>
        <td>Latent Codes</td>
        <td>GAN based</td>
        <td>CVPR 2024</td>
    </tr>
  <tr>
        <td>BrushNet</td>
        <td>Diffusion based</td>
        <td>ECCV 2024</td>
    </tr>
  <tr>
        <td>PILOT</td>
        <td>Diffusion based</td>
        <td>Arxiv 2024</td>
    </tr>
  <tr>
        <td>iLAT</td>
        <td>AR based</td>
        <td>NIPS 2021</td>
    </tr>
  <tr>
        <td>FaceVAE</td>
        <td>VAE based</td>
        <td>ENIAC 2021</td>
    </tr>
  <tr>
        <td>GPEN</td>
        <td>GAN-based</td>
        <td>Arxiv 2021</td>
    </tr>
  <tr>
        <td>VToonify</td>
        <td>GAN-based</td>
        <td>ACM MM 2022</td>
    </tr>
  <tr>
        <td>HFGI</td>
        <td>GAN-based</td>
        <td>CVPR 2022</td>
    </tr>
  <tr>
        <td>PSP</td>
        <td>GAN-based</td>
        <td>CVPR 2021</td>
    </tr>
  <tr>
        <td rowspan="17">FR</td>
        <td>FOMM</td>
        <td>GAN-based</td>
        <td>NIPS 2019</td>
    </tr>
  <tr>
        <td>DreamTalk</td>
        <td>Diffusion based</td>
        <td>Arxiv 2023</td>
    </tr>
  <tr>
        <td>FS_vid2vid</td>
        <td>GAN-based</td>
        <td>Arxiv 2019</td>
    </tr>
  <tr>
        <td>Wav2Lip</td>
        <td>GAN-based</td>
        <td>ACM MM 2020</td>
    </tr>
  <tr>
        <td>TPSMM</td>
        <td>GAN-based</td>
        <td>CVPR 2022</td>
    </tr>
  <tr>
        <td>DaGAN</td>
        <td>GAN-based</td>
        <td>CVPR 2022</td>
    </tr>
  <tr>
        <td>LIA</td>
        <td>GAN-based</td>
        <td>ICLR 2022</td>
    </tr>
  <tr>
        <td>SadTalker</td>
        <td>Diffusion based</td>
        <td>CVPR 2023</td>
    </tr>
  <tr>
        <td>FADM</td>
        <td>Diffusion based</td>
        <td>CVPRW 2023</td>
    </tr>
  <tr>
        <td>EDTalk</td>
        <td>Diffusion based</td>
        <td>ECCV 2024</td>
    </tr>
  <tr>
        <td>deep_3drecon</td>
        <td>NeRF based</td>
        <td>CVPR 2019</td>
    </tr>
  <tr>
        <td>MCNet</td>
        <td>GAN based</td>
        <td>ICCV 2023</td>
    </tr>
  <tr>
        <td>LivePortrait</td>
        <td>GAN based</td>
        <td>Arxiv 2024</td>
    </tr>
  <tr>
        <td>MagicAnimate</td>
        <td>Diffusion based</td>
        <td>CVPR 2024</td>
    </tr>
  <tr>
        <td>FreeViewTalk</td>
        <td>GAN based</td>
        <td>CVPR 2021</td>
    </tr>
  <tr>
        <td>AniPortrait</td>
        <td>Diffusion based</td>
        <td>Arxiv 2024</td>
    </tr>
  <tr>
        <td>VideoRetalking</td>
        <td>GAN based</td>
        <td>SIGGRAPH Asia 2022</td>
    </tr>
  <tr>
        <td rowspan="7">TTS</td>
        <td>Yourtts</td>
        <td>VAE based</td>
        <td>PMLR 2022</td>
    </tr>
  <tr>
        <td>Chattts</td>
        <td>GAN based</td>
        <td>Unknown</td>
    </tr>
  <tr>
        <td>F5-TTS</td>
        <td>Diffusion based</td>
        <td>Arxiv 2024</td>
    </tr>
  <tr>
        <td>MARS5_TTS</td>
        <td>Diffusion based</td>
        <td>Unknown</td>
    </tr>
  <tr>
        <td>VITS-VCTK</td>
        <td>VAE based</td>
        <td>PMLR 2021</td>
    </tr>
  <tr>
        <td>GPT-SoVITS</td>
        <td>GAN based</td>
        <td>Arxiv 2024</td>
    </tr>
    <tr>
        <td>Xtts</td>
        <td>VAE based</td>
        <td>Arxiv 2024</td>
    </tr>
    <tr>
        <td rowspan="2">VC</td>
        <td>VALL-E</td>
        <td>GAN-based</td>
        <td>Arxiv 2023</td>
    </tr>
    <tr>
        <td>OpenVoice</td>
        <td>GAN-based</td>
        <td>Arxiv 2023</td>
    </tr>
  <tr>
        <td rowspan="4">VC</td>
        <td>VidMuse</td>
        <td>Diffusion based</td>
        <td>CVPR 2025</td>
    </tr>
    <tr>
        <td>AudioX</td>
        <td>Diffusion based</td>
        <td>Arxiv 2025</td>
    </tr>
    <tr>
        <td>MMAudio</td>
        <td>GAN-based</td>
        <td>CVPR 2025</td>
    </tr>
    <tr>
        <td>FoleyCrafter</td>
        <td>Diffusion based</td>
        <td>Arxiv 2024</td>
    </tr>
</table>
