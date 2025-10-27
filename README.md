# CS330
Computer graphics and visualization using OpenGL
Cs 330 Module Eight Journal
SNHU
Kimberly Blackwood
Professor Wilson
October 22,2025













How do you approach designing software?
Designing a software system is an important skill for software developers. It involves planning and structuring complex systems to meet specific requirements, ensuring scalability, reliability, efficiency, and maintainability. It is vital to create robust and adaptable systems that can handle real-world demands and future growth. Below are steps I will take to designing a software system:

•	Define the problem -  understanding the requirements, goals, and constraints of the system
•	Requirements gathering and planning – information is gathered, analyzed, and documented. Teams collaborate on what needs to be done.
•	Creating a design – a detailed blueprint of the requirement is created
•	Implementation – code is written based on the design needs
•	Testing –  test is conducted to ensure they meet requirements and quality standards.
What new design skills has your work on the project helped you to craft?
Using OpenGL has given me the opportunity to create both 2D and 3D visualizations by manipulating texture, vertices, and shaders.  This experience  strength my understandings of graphic concepts for example, how to organize the rendering operation and coordinate transformation to achieve realistic visual effects. 
What design process did you follow for your project work?
My design process followed a structured sequence to ensure clarity and efficacy as described below:
•	First, I reviewed the module resources., to familiarize myself with the project expectation
•	Then, I downloaded the CS330Content ZIP file and extract the contents to the C drive on my  system. 
•	Once I understand the content, I begin the assignment by opening the Visual Studio solution (SLN) file. 
•	Then the Visual Studio application automatically launched. 
•	I made sure to use the 2022 version of Visual Studio. 
•	Then I build and run the project. 
•	I then created code to address the required functionality. 
•	I then ensured that the code met the required functionality and visual representation outlined for the assignment. 
•	My approach involved iterative development, writing small portions of the code, running test, debugging, and refining until the functionality and appearance were satisfactory.
•	I made sure to apply proper logical structuring  and  correct syntax to code. 
•	I followed best practices by applying proper formatting standards and comprehensive commenting to make the code readable and maintainable.
How could tactics from your design approach be applied in future work?
In the future, the tactic of breaking the code down in little parts building it up slowly will help me to create codes where it is easy to identify and fix errors by building and running the code frequently after every block of code. This approach will help me maintain  high quality of work and reduce development time.
How do you approach developing programs?
Here is how I approach developing a program:
•	Create requirement specification.
•	Prepare project plan.
•	Design user experience and user interface.
•	Architect the software.
•	Code the solution.
•	Establish integrations.
•	Run rigorous testing.
•	Deploy program.
What new development strategies did you use while working on your 3D scene?
A new development strategy that I used while working on the 3D scene was, UV mapping and texturing. In this strategy the 3D model surface was unwrapped into a flat 2D plane to accurately apply texture. By assigning the UV coordinate textures can be  projected onto the complex 3D shape correctly without distortion and stretching. I used Physically Based Rendering  (PBR) textures to ensure materials react to light realistically.
How did iteration factor into your development?
It allowed me to build and improve my scene incrementally. Which allowed me to adjust slowly to lighting, textures, and shapes. This enabled me to catch mistakes early in code development.
How has your approach to developing code evolved throughout the milestones, which led you to the project’s completion?
Completing each milestone contributed to my patience and testing abilities. With each milestone I was able to move one step further into the final project.  This resulted in me being attentive and not rush the code development process. I learned that it is better to put a little code at a time to prevent errors.
How can computer science help me in reaching my goals?
Computer science will help me to reach my goal of becoming a software engineer by developing my problem-solving skills which is done through critical thinking. It taught me the importance of  being adaptable, learning new technology, and methodology. This is important to be established  in the field for a long term.
10.How do computational graphics and visualizations give you new knowledge and skills that can be applied in your future educational pathway?
Computational graphics and visualization gave me new knowledge and skill such as: making the data easier to understand, by improving my comprehension of complex systems.
<img width="468" height="666" alt="image" src="https://github.com/user-attachments/assets/d41dd649-5d89-40b5-b23e-e385ed1ff9f4" />




CS 330 Milestone One
SNHU
Kimberly Blackwood
Professor R. Wilson
September 4,2025
































For this project I took a photo of a lemon tree in a flowerpot,  placed on a grassy patch with a lantern and a garden shovel nearby. This scene will allow me to show basic  3D shapes.  The trunk of the tree will be modeled as a cylinder, lemons as a sphere, the canopy as  a cone,  flowerpot as a tapered cylinder. The dirt and grass are modeled as a plane. The lantern will be  shown as a  rectangular box, the garden shovel combines two different shapes cylinder for the handle and prism for the blade.

I choose these objects because they will demonstrate how we can combine  basic shapes into different forms to create recognizable forms. The approach I followed made it less complex to achieve the requirements. I incorporate forms with minimal complexity which made the work achievable.



Summary Table (for clarity)

Object	Basic 3D Shapes	Explanation
Lemon Tree	Sphere, cylinder, cone	Thre tree trunk can be a cylinder for the height, the lemons as a sphere for their rounded appearance and the canopy as a cone for its peak and spread base.
Flower Pot	Tapered cylinder	The flower pot with the sloping sides will be best captured as  a tapered cylinder.
Grass	Plane	A flat plane will serve as the ground and on it the grass blades will be represented by coloring it green.
Dirt	Plane	The dirt inside the flower pot will be hirizontal and colored brown.
Lanttern	Box	The lanter rectangular form will best match an elongated box.
Shovel	Prism and cylinder 	The handle of the shovel will be a cylinder because it has a round shape and the blade will be a prism because it is flat and angular.


 
///////////////////////////////////////////////////////////////////////////////
// shadermanager.cpp
// ============
// manage the loading and rendering of 3D scenes
//
//  AUTHOR: Brian Battersby - SNHU Instructor / Computer Science
//	Created for CS-330-Computational Graphics and Visualization, Nov. 1st, 2023
///////////////////////////////////////////////////////////////////////////////

#include "SceneManager.h"

#ifndef STB_IMAGE_IMPLEMENTATION
#define STB_IMAGE_IMPLEMENTATION
#include "stb_image.h"
#endif

#include <glm/gtx/transform.hpp>

// declaration of global variables
namespace
{
	const char* g_ModelName = "model";
	const char* g_ColorValueName = "objectColor";
	const char* g_TextureValueName = "objectTexture";
	const char* g_UseTextureName = "bUseTexture";
	const char* g_UseLightingName = "bUseLighting";
}

/***********************************************************
 *  SceneManager()
 *
 *  The constructor for the class
 ***********************************************************/
SceneManager::SceneManager(ShaderManager *pShaderManager)
{
	m_pShaderManager = pShaderManager;
	m_basicMeshes = new ShapeMeshes();

	// initialize the texture collection
	for (int i = 0; i < 16; i++)
	{
		m_textureIDs[i].tag = "/0";
		m_textureIDs[i].ID = -1;
	}
	m_loadedTextures = 0;
}

/***********************************************************
 *  ~SceneManager()
 *
 *  The destructor for the class
 ***********************************************************/
SceneManager::~SceneManager()
{
	m_pShaderManager = NULL;
	delete m_basicMeshes;
	m_basicMeshes = NULL;
	// destroy the created OpenGL textures
	DestroyGLTextures();
}

/***********************************************************
 *  CreateGLTexture()
 *
 *  This method is used for loading textures from image files,
 *  configuring the texture mapping parameters in OpenGL,
 *  generating the mipmaps, and loading the read texture into
 *  the next available texture slot in memory.
 ***********************************************************/
bool SceneManager::CreateGLTexture(const char* filename, std::string tag)
{
	int width = 0;
	int height = 0;
	int colorChannels = 0;
	GLuint textureID = 0;

	// indicate to always flip images vertically when loaded
	stbi_set_flip_vertically_on_load(true);

	// try to parse the image data from the specified image file
	unsigned char* image = stbi_load(
		filename,
		&width,
		&height,
		&colorChannels,
		0);

	// if the image was successfully read from the image file
	if (image)
	{
		std::cout << "Successfully loaded image:" << filename << ", width:" << width << ", height:" << height << ", channels:" << colorChannels << std::endl;

		glGenTextures(1, &textureID);
		glBindTexture(GL_TEXTURE_2D, textureID);

		// set the texture wrapping parameters
		glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
		glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
		// set texture filtering parameters
		glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
		glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

		// if the loaded image is in RGB format
		if (colorChannels == 3)
			glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB8, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, image);
		// if the loaded image is in RGBA format - it supports transparency
		else if (colorChannels == 4)
			glTexImage2D(GL_TEXTURE_2D, 0, GL_RGBA8, width, height, 0, GL_RGBA, GL_UNSIGNED_BYTE, image);
		else
		{
			std::cout << "Not implemented to handle image with " << colorChannels << " channels" << std::endl;
			return false;
		}

		// generate the texture mipmaps for mapping textures to lower resolutions
		glGenerateMipmap(GL_TEXTURE_2D);

		// free the image data from local memory
		stbi_image_free(image);
		glBindTexture(GL_TEXTURE_2D, 0); // Unbind the texture

		// register the loaded texture and associate it with the special tag string
		m_textureIDs[m_loadedTextures].ID = textureID;
		m_textureIDs[m_loadedTextures].tag = tag;
		m_loadedTextures++;

		return true;
	}

	std::cout << "Could not load image:" << filename << std::endl;

	// Error loading the image
	return false;
}

/***********************************************************
 *  BindGLTextures()
 *
 *  This method is used for binding the loaded textures to
 *  OpenGL texture memory slots.  There are up to 16 slots.
 ***********************************************************/
void SceneManager::BindGLTextures()
{
	for (int i = 0; i < m_loadedTextures; i++)
	{
		// bind textures on corresponding texture units
		glActiveTexture(GL_TEXTURE0 + i);
		glBindTexture(GL_TEXTURE_2D, m_textureIDs[i].ID);
	}
}

/***********************************************************
 *  DestroyGLTextures()
 *
 *  This method is used for freeing the memory in all the
 *  used texture memory slots.
 ***********************************************************/
void SceneManager::DestroyGLTextures()
{
	for (int i = 0; i < m_loadedTextures; i++)
	{
		glGenTextures(1, &m_textureIDs[i].ID);
	}
}

/***********************************************************
 *  FindTextureID()
 *
 *  This method is used for getting an ID for the previously
 *  loaded texture bitmap associated with the passed in tag.
 ***********************************************************/
int SceneManager::FindTextureID(std::string tag)
{
	int textureID = -1;
	int index = 0;
	bool bFound = false;

	while ((index < m_loadedTextures) && (bFound == false))
	{
		if (m_textureIDs[index].tag.compare(tag) == 0)
		{
			textureID = m_textureIDs[index].ID;
			bFound = true;
		}
		else
			index++;
	}

	return(textureID);
}

/***********************************************************
 *  FindTextureSlot()
 *
 *  This method is used for getting a slot index for the previously
 *  loaded texture bitmap associated with the passed in tag.
 ***********************************************************/
int SceneManager::FindTextureSlot(std::string tag)
{
	int textureSlot = -1;
	int index = 0;
	bool bFound = false;

	while ((index < m_loadedTextures) && (bFound == false))
	{
		if (m_textureIDs[index].tag.compare(tag) == 0)
		{
			textureSlot = index;
			bFound = true;
		}
		else
			index++;
	}

	return(textureSlot);
}

/***********************************************************
 *  FindMaterial()
 *
 *  This method is used for getting a material from the previously
 *  defined materials list that is associated with the passed in tag.
 ***********************************************************/
bool SceneManager::FindMaterial(std::string tag, OBJECT_MATERIAL& material)
{
	if (m_objectMaterials.size() == 0)
	{
		return(false);
	}

	int index = 0;
	bool bFound = false;
	while ((index < m_objectMaterials.size()) && (bFound == false))
	{
		if (m_objectMaterials[index].tag.compare(tag) == 0)
		{
			bFound = true;
			material.ambientColor = m_objectMaterials[index].ambientColor;
			material.ambientStrength = m_objectMaterials[index].ambientStrength;
			material.diffuseColor = m_objectMaterials[index].diffuseColor;
			material.specularColor = m_objectMaterials[index].specularColor;
			material.shininess = m_objectMaterials[index].shininess;
		}
		else
		{
			index++;
		}
	}

	return(true);
}

/***********************************************************
 *  SetTransformations()
 *
 *  This method is used for setting the transform buffer
 *  using the passed in transformation values.
 ***********************************************************/
void SceneManager::SetTransformations(
	glm::vec3 scaleXYZ,
	float XrotationDegrees,
	float YrotationDegrees,
	float ZrotationDegrees,
	glm::vec3 positionXYZ)
{
	// variables for this method
	glm::mat4 modelView;
	glm::mat4 scale;
	glm::mat4 rotationX;
	glm::mat4 rotationY;
	glm::mat4 rotationZ;
	glm::mat4 translation;

	// set the scale value in the transform buffer
	scale = glm::scale(scaleXYZ);
	// set the rotation values in the transform buffer
	rotationX = glm::rotate(glm::radians(XrotationDegrees), glm::vec3(1.0f, 0.0f, 0.0f));
	rotationY = glm::rotate(glm::radians(YrotationDegrees), glm::vec3(0.0f, 1.0f, 0.0f));
	rotationZ = glm::rotate(glm::radians(ZrotationDegrees), glm::vec3(0.0f, 0.0f, 1.0f));
	// set the translation value in the transform buffer
	translation = glm::translate(positionXYZ);

	modelView = translation * rotationX * rotationY * rotationZ * scale;

	if (NULL != m_pShaderManager)
	{
		m_pShaderManager->setMat4Value(g_ModelName, modelView);
	}
}

/***********************************************************
 *  SetShaderColor()
 *
 *  This method is used for setting the passed in color
 *  into the shader for the next draw command
 ***********************************************************/
void SceneManager::SetShaderColor(
	float redColorValue,
	float greenColorValue,
	float blueColorValue,
	float alphaValue)
{
	// variables for this method
	glm::vec4 currentColor;

	currentColor.r = redColorValue;
	currentColor.g = greenColorValue;
	currentColor.b = blueColorValue;
	currentColor.a = alphaValue;

	if (NULL != m_pShaderManager)
	{
		m_pShaderManager->setIntValue(g_UseTextureName, false);
		m_pShaderManager->setVec4Value(g_ColorValueName, currentColor);
	}
}

/***********************************************************
 *  SetShaderTexture()
 *
 *  This method is used for setting the texture data
 *  associated with the passed in ID into the shader.
 ***********************************************************/
void SceneManager::SetShaderTexture(
	std::string textureTag)
{
	if (NULL != m_pShaderManager)
	{
		m_pShaderManager->setIntValue(g_UseTextureName, true);

		int textureID = -1;
		textureID = FindTextureSlot(textureTag);
		m_pShaderManager->setSampler2DValue(g_TextureValueName, textureID);
	}
}

/***********************************************************
 *  SetTextureUVScale()
 *
 *  This method is used for setting the texture UV scale
 *  values into the shader.
 ***********************************************************/
void SceneManager::SetTextureUVScale(float u, float v)
{
	if (NULL != m_pShaderManager)
	{
		m_pShaderManager->setVec2Value("UVscale", glm::vec2(u, v));
	}
}

/***********************************************************
 *  SetShaderMaterial()
 *
 *  This method is used for passing the material values
 *  into the shader.
 ***********************************************************/
void SceneManager::SetShaderMaterial(
	std::string materialTag)
{
	if (m_objectMaterials.size() > 0)
	{
		OBJECT_MATERIAL material;
		bool bReturn = false;

		bReturn = FindMaterial(materialTag, material);
		if (bReturn == true)
		{
			m_pShaderManager->setVec3Value("material.ambientColor", material.ambientColor);
			m_pShaderManager->setFloatValue("material.ambientStrength", material.ambientStrength);
			m_pShaderManager->setVec3Value("material.diffuseColor", material.diffuseColor);
			m_pShaderManager->setVec3Value("material.specularColor", material.specularColor);
			m_pShaderManager->setFloatValue("material.shininess", material.shininess);
		}
	}
}

/**************************************************************/
/*** STUDENTS CAN MODIFY the code in the methods BELOW for  ***/
/*** preparing and rendering their own 3D replicated scenes.***/
/*** Please refer to the code in the OpenGL sample project  ***/
/*** for assistance.                                        ***/
/**************************************************************/

/***********************************************************
  *  LoadSceneTextures()
  *
  *  This method is used for preparing the 3D scene by loading
  *  the shapes, textures in memory to support the 3D scene
  *  rendering
  ***********************************************************/
void SceneManager::LoadSceneTextures()
{
	/*** STUDENTS - add the code BELOW for loading the textures that ***/
	/*** will be used for mapping to objects in the 3D scene. Up to  ***/
	/*** 16 textures can be loaded per scene. Refer to the code in   ***/
	/*** the OpenGL Sample for help.                                 ***/

	// laying out coordinates for textures

	CreateGLTexture("Utilities/textures/rusticwood.jpg", "rwood");
	CreateGLTexture("Utilities/textures/stainless_end.jpg", "blade");
	CreateGLTexture("Utilities/textures/knife_handle.jpg", "head");
	CreateGLTexture("Utilities/textures/green_grass.jpeg", "floor");
	CreateGLTexture("Utilities/textures/cheddar.jpg", "lemon");
	CreateGLTexture("Utilities/textures/circular-brushed-gold-textureｾ.jpg", "lantern");


	


	// after the texture image data is loaded into memory, the
	// loaded textures need to be bound to texture slots - there
	// are a total of 16 available slots for scene textures
	BindGLTextures();
}


/***********************************************************
  *  DefineObjectMaterials()
  *
  *  This method is used for configuring the various material
  *  settings for ｾall of the objects within the 3D scene.
  ***********************************************************/
void SceneManager::DefineObjectMaterials()

{
	/*** STUDENTS - add the code BELOW for defining object materials. ***/
	/*** There is no limit to the number of object materials that can ***/
	/*** be defined. Refer to the code in the OpenGL Sample for help  ***/

	//Define shiny gold material 
	OBJECT_MATERIAL goldMaterial;
	goldMaterial.ambientColor = glm::vec3(0.2f, 0.2f, 0.1f);
	goldMaterial.ambientStrength = 0.4f;
	goldMaterial.diffuseColor = glm::vec3(0.3f, 0.3f, 0.2f);
	goldMaterial.specularColor = glm::vec3(0.6f, 0.5f, 0.4f);
	goldMaterial.shininess = 22.0;
	goldMaterial.tag = "gold";
	m_objectMaterials.push_back(goldMaterial);


	//Define natural wood material
	OBJECT_MATERIAL woodMaterial;
	woodMaterial.ambientColor = glm::vec3(0.4f, 0.3f, 0.1f);
	woodMaterial.ambientStrength = 0.2f;
	woodMaterial.diffuseColor = glm::vec3(0.3f, 0.2f, 0.1f);
	woodMaterial.specularColor = glm::vec3(0.1f, 0.1f, 0.1f);
	woodMaterial.shininess = 0.3;
	woodMaterial.tag = "wood";
	m_objectMaterials.push_back(woodMaterial);

	OBJECT_MATERIAL cementMaterial;
	cementMaterial.ambientColor = glm::vec3(0.2f, 0.2f, 0.2f);
	cementMaterial.ambientStrength = 0.2f;
	cementMaterial.diffuseColor = glm::vec3(0.5f, 0.5f, 0.5f);
	cementMaterial.specularColor = glm::vec3(0.4f, 0.4f, 0.4f);
	cementMaterial.shininess = 0.5;
	cementMaterial.tag = "cement";
	m_objectMaterials.push_back(cementMaterial);

	OBJECT_MATERIAL tileMaterial;
	tileMaterial.ambientColor = glm::vec3(0.2f, 0.3f, 0.4f);
	tileMaterial.ambientStrength = 0.3f;
	tileMaterial.diffuseColor = glm::vec3(0.3f, 0.2f, 0.1f);
	tileMaterial.specularColor = glm::vec3(0.4f, 0.5f, 0.6f);
	tileMaterial.shininess = 25.0;
	tileMaterial.tag = "tile";
	m_objectMaterials.push_back(tileMaterial);

	OBJECT_MATERIAL glassMaterial;
	glassMaterial.ambientColor = glm::vec3(0.4f, 0.4f, 0.4f);
	glassMaterial.ambientStrength = 0.3f;
	glassMaterial.diffuseColor = glm::vec3(0.3f, 0.3f, 0.3f);
	glassMaterial.specularColor = glm::vec3(0.6f, 0.6f, 0.6f);
	glassMaterial.shininess = 85.0;
	glassMaterial.tag = "glass";
	m_objectMaterials.push_back(glassMaterial);
	
	OBJECT_MATERIAL clayMaterial;
	clayMaterial.ambientColor = glm::vec3(0.2f, 0.2f, 0.3f);
	clayMaterial.ambientStrength = 0.3f;
	clayMaterial.diffuseColor = glm::vec3(0.4f, 0.4f, 0.5f);
	clayMaterial.specularColor = glm::vec3(0.2f, 0.2f, 0.4f);
	clayMaterial.shininess = 0.5;
	clayMaterial.tag = "clay";
	m_objectMaterials.push_back(clayMaterial);


}

/***********************************************************
 *  SetupSceneLights()
 *
 *  This method is called to add and configure the light
 *  sources for the 3D scene.  There are up to 4 light sources.
 ***********************************************************/
void SceneManager::SetupSceneLights()
{
	// this line of code is NEEDED for telling the shaders to render 
	// the 3D scene with custom lighting, if no light sources have
	// been added then the display window will be black - to use the 
	// default OpenGL lighting then comment out the following line
	m_pShaderManager->setBoolValue(g_UseLightingName, true);

	/*** STUDENTS - add the code BELOW for setting up light sources ***/
	/*** Up to four light sources can be defined. Refer to the code ***/
	/*** in the OpenGL Sample for help                              ***/

	m_pShaderManager->setBoolValue("bUseLighting", true);


	//light source combinations, three source for the lighting
	//LIGHT (0) WHITE POINT
	m_pShaderManager->setVec3Value("lightSources[0].position", 3.0f, 7.0f, 12.0f);
	m_pShaderManager->setVec3Value("lightSources[0].ambientColor", 1.0f, 1.0f,1.0f);
	m_pShaderManager->setVec3Value("lightSources[0].diffuseColor", 1.4f, 1.4f, 1.4f);
	m_pShaderManager->setVec3Value("lightSources[0].specularColor", 1.0f, 1.0f, 1.0f);
	m_pShaderManager->setFloatValue("lightSources[0].focalStrength", 16.0f);
	m_pShaderManager->setFloatValue("lightSources[0].specularIntensity", 0.5f);

	//Light (1) colored spotlight
	m_pShaderManager->setVec3Value("lightSources[1].position", -3.0f, 9.0f, 6.0f);
	m_pShaderManager->setVec3Value("lightSources[1].ambientColor", 0.1f, 0.08f, 0.03f);
	m_pShaderManager->setVec3Value("lightSources[1].diffuseColor", 0.9f, 0.8f, 0.5f);
	m_pShaderManager->setVec3Value("lightSources[1].specularColor", 1.0f, 0.75f, 0.2f);
	m_pShaderManager->setFloatValue("lightSources[1].focalStrength", 50.0f);
	m_pShaderManager->setFloatValue("lightSources[1].specularIntensity", 0.8f);

	//Light (2) rim lighting
	m_pShaderManager->setVec3Value("lightSources[2].position", 0.0f, 5.0f, 10.0f);
	m_pShaderManager->setVec3Value("lightSources[2].ambientColor", 0.02f, 0.02f, 0.02f);
	m_pShaderManager->setVec3Value("lightSources[2].diffuseColor", 0.3f, 0.3f, 0.3f);
	m_pShaderManager->setVec3Value("lightSources[2].specularColor", 0.1f, 0.1f, 0.1f);
	m_pShaderManager->setFloatValue("lightSources[2].focalStrength", 13.0f);
	m_pShaderManager->setFloatValue("lightSources[2].specularIntensity", 0.2f);



}



/***********************************************************
 *  PrepareScene()
 *
 *  This method is used for preparing the 3D scene by loading
 *  the shapes, textures in memory to support the 3D scene 
 *  rendering
 ***********************************************************/
void SceneManager::PrepareScene()
{
	// only one instance of a particular mesh needs to be
	// loaded in memory no matter how many times it is drawn
	// in the rendered 3D scene

	m_basicMeshes->LoadPlaneMesh();
	m_basicMeshes->LoadCylinderMesh();
	m_basicMeshes->LoadPrismMesh();
	m_basicMeshes->LoadTorusMesh();
	m_basicMeshes->LoadTaperedCylinderMesh();
	m_basicMeshes->LoadSphereMesh();



	// load the textures for the 3D scene
	LoadSceneTextures();

	// define the materials for objects in the scene
	DefineObjectMaterials();
	// add and define the light sources for the scene
	SetupSceneLights();


}
/***********************************************************
 *  RenderScene()
 *
 *  This method is used for rendering the 3D scene by 
 *  transforming and drawing the basic 3D shapes
 ***********************************************************/
void SceneManager::RenderScene()
{
	// declare the variables for the transformations
	glm::vec3 scaleXYZ;
	float XrotationDegrees = 0.0f;
	float YrotationDegrees = 0.0f;
	float ZrotationDegrees = 0.0f;
	glm::vec3 positionXYZ;

	/*** Set needed transformations before drawing the basic mesh.  ***/
	/*** This same ordering of code should be used for transforming ***/
	/*** and drawing all the basic 3D shapes.						***/
	/******************************************************************/
	// set the XYZ scale for the mesh
	scaleXYZ = glm::vec3(20.0f, 1.0f, 10.0f);

	// set the XYZ rotation for the mesh
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the mesh
	positionXYZ = glm::vec3(0.0f, 0.0f, 0.0f);

	// set the transformations into memory to be used on the drawn meshes
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);
	
	//add color, texture and texture scale to image
		//SetShaderColor(.3,.4, .1, 1);
		SetShaderTexture("floor");
		SetTextureUVScale(1.0, 1.0);

		//add lighting effects
		SetShaderMaterial("tile");


	// draw the mesh with transformation values, this is the ground 
	m_basicMeshes->DrawPlaneMesh();

	// set the XYZ scale for the cylinder
	scaleXYZ = glm::vec3(1.0f, 8.0f, 0.0f);

	// set the XYZ rotation for the cylinder
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 90.0f;

	// set the XYZ position for the cylinder
	positionXYZ = glm::vec3(5.0f, 0.0f, 0.0f);

	// set the transformations into memory to be used on the drawn meshes
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);
	//add color, texture and texture scale to image
	SetShaderColor(0.65f, 0.17f, 0.17f, 1);
	SetShaderTexture("rwood");
	SetTextureUVScale(1.0, 1.0);
	SetShaderMaterial("wood");
	
	// draw the mesh with transformation values, shovel body
	m_basicMeshes->DrawCylinderMesh();

	// set the XYZ scale for the prism
	scaleXYZ = glm::vec3(2.0f, 2.0f, 2.0f);

	// set the XYZ rotation for the prism
	XrotationDegrees = 0.0f;
	YrotationDegrees = 90.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the prism
	positionXYZ = glm::vec3(6.0f, 0.0f, 0.0f);

	// set the transformations into memory to be used on the drawn meshes
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("blade");
	SetTextureUVScale(1.0, 1.0);

	// add lighting effects
	SetShaderMaterial("gold");


	// draw the mesh with transformation values, blade of shovel
	m_basicMeshes->DrawPrismMesh();

	// set the XYZ scale for the torus
	scaleXYZ = glm::vec3(1.0f, 1.0f,1.0f);

	// set the XYZ rotation for the torus
	XrotationDegrees = 90.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the torus
	positionXYZ = glm::vec3(-2.8f, 2.0f, 4.0f);

	// set the transformations into memory to be used on the drawn torus
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("head");
	SetTextureUVScale(1.0, 1.0);

	//add lighting effects
	SetShaderMaterial("clay");


	// draw the mesh with transformation values, handle of shovel
	m_basicMeshes->DrawTorusMesh();

	// set the XYZ scale for the tapered cylinder
	scaleXYZ = glm::vec3(2.0f, 1.0f, 1.0f);

	// set the XYZ rotation for the tapered cylinder
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 180.0f;

	// set the XYZ position for the tapered cylinder
	positionXYZ = glm::vec3(1.5f, 0.2f, 4.0f);

	// set the transformations into memory to be used on the drawn tapered cylinder
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("rwood");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("cement");

	// draw the mesh with transformation values
	m_basicMeshes->DrawTaperedCylinderMesh();

	// set the XYZ scale for the tapered cylinder
	scaleXYZ = glm::vec3(1.0f, 2.0f, 1.0f);

	// set the XYZ rotation for the tapered cylinder
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the tapered cylinder
	positionXYZ = glm::vec3(-5.5f, 0.2f, 4.0f);

	// set the transformations into memory to be used on the drawn tapered cylinder
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("lantern");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("gold");

	// draw the mesh with transformation values for lantern
	m_basicMeshes->DrawTaperedCylinderMesh();

	// set the XYZ scale for the cylinder for tree trunk
	scaleXYZ = glm::vec3(1.0f, 60.0f, 1.0f);

	// set the XYZ rotation for the cylinder
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 180.0f;

	// set the XYZ position for the cylinder
	positionXYZ = glm::vec3(1.5f, 5.0f, 4.0f);

	// set the transformations into memory to be used on the drawn cylinder
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("rwood");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("wood");

	// draw the mesh with transformation values, for tree trunk
	m_basicMeshes->DrawCylinderMesh();

	// set the XYZ scale for the sphere
	scaleXYZ = glm::vec3(3.0f, 3.0f, 3.0f);

	// set the XYZ rotation for the sphere
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the tapered sphere
	positionXYZ = glm::vec3(1.5f, 6.0f, 4.0f);

	// set the transformations into memory to be used on the drawn sphere
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("floor");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	 SetShaderMaterial("clay");

	// draw the mesh with transformation values, for tree canopy
	m_basicMeshes->DrawSphereMesh();

	// set the XYZ scale for the sphere
	scaleXYZ = glm::vec3(0.2f, 0.2f, 0.2f);

	// set the XYZ rotation for the sphere
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the sphere
	positionXYZ = glm::vec3(0.0f, 5.0f, 10.0f);

	// set the transformations into memory to be used on the drawn sphere
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("lemon");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("gold");

	// draw the mesh with transformation values, for lemon
	m_basicMeshes->DrawSphereMesh();

	// set the XYZ scale for the sphere
	scaleXYZ = glm::vec3(0.2f, 0.2f, 0.2f);

	// set the XYZ rotation for the sphere
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the sphere
	positionXYZ = glm::vec3(0.5f, 5.0f, 10.0f);

	// set the transformations into memory to be used on the drawn sphere
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("lemon");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("gold");

	// draw the mesh with transformation values, for 2nd lemon from left
	m_basicMeshes->DrawSphereMesh();

	// set the XYZ scale for the sphere
	scaleXYZ = glm::vec3(0.2f, 0.2f, 0.2f);

	// set the XYZ rotation for the sphere
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the sphere
	positionXYZ = glm::vec3(1.0f, 5.0f, 10.0f);

	// set the transformations into memory to be used on the drawn sphere
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("lemon");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	 SetShaderMaterial("gold");

	// draw the mesh with transformation values,for 3rd lemon from left 
	m_basicMeshes->DrawSphereMesh();

	// set the XYZ scale for the sphere
	scaleXYZ = glm::vec3(0.2f, 0.2f, 0.2f);

	// set the XYZ rotation for the sphere
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the sphere
	positionXYZ = glm::vec3(0.6f, 5.5f, 10.0f);

	// set the transformations into memory to be used on the drawn sphere
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("lemon");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("gold");

	// draw the mesh with transformation values, for 1st top left  lemon
	m_basicMeshes->DrawSphereMesh();

	// set the XYZ scale for the sphere
	scaleXYZ = glm::vec3(0.2f, 0.2f, 0.2f);

	// set the XYZ rotation for the sphere
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the sphere
	positionXYZ = glm::vec3(0.1f, 5.5f, 10.0f);

	// set the transformations into memory to be used on the drawn sphere
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("lemon");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("gold");

	// draw the mesh with transformation values, for 2nd top left  lemon
	m_basicMeshes->DrawSphereMesh();

	// set the XYZ scale for the sphere
	scaleXYZ = glm::vec3(0.2f, 0.2f, 0.2f);

	// set the XYZ rotation for the sphere
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the sphere
	positionXYZ = glm::vec3(0.8f, 6.0f, 10.0f);

	// set the transformations into memory to be used on the drawn sphere
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("lemon");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("gold");

	// draw the mesh with transformation values, for 1st top left lemon
	m_basicMeshes->DrawSphereMesh();

	// set the XYZ scale for the sphere
	scaleXYZ = glm::vec3(0.2f, 0.2f, 0.2f);

	// set the XYZ rotation for the sphere
	XrotationDegrees = 0.0f;
	YrotationDegrees = 0.0f;
	ZrotationDegrees = 0.0f;

	// set the XYZ position for the sphere
	positionXYZ = glm::vec3(0.3f, 6.0f, 10.0f);

	// set the transformations into memory to be used on the drawn sphere
	SetTransformations(
		scaleXYZ,
		XrotationDegrees,
		YrotationDegrees,
		ZrotationDegrees,
		positionXYZ);

	//add color, texture and texture scale to image
	//SetShaderColor(0.55f, 0.55f, 0.55f, 1);
	SetShaderTexture("lemon");
	SetTextureUVScale(1.0, 1.0);
	//add lighting effects
	SetShaderMaterial("gold");

	// draw the mesh with transformation values, for 1st top left lemon
	m_basicMeshes->DrawSphereMesh();


}

	/****************************************************************/ 

 
 

